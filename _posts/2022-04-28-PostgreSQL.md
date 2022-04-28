---
layout: post
title: PostgreSQL
category: install steps
tags: Ruby on Rails
---

### Switch SQLite3 to PostgreSQL

## Database

database是資料庫，為什麼需要資料庫？因為 client - server  互動需要資料傳輸， client 傳送資料給 server ， server 向 client 顯示資料，或是儲存 client 傳送的資料。所以需要 database 儲存資料。

假如所有資料通通放在一起，沒有一個系統性的整理，會變得很可怕。所以我們可以利用表格的方式整理散亂的資料，table 是資料表格，fields column 是"欄" ，record row 是資料"列"，視覺呈現會是這樣

![table](/assets/post_img/sql-table-column-row.png "table-column-row")

## DBMS

database management system 管理資料庫的系統。MySQL / PostgreSQL

## SQL

操作資料庫的語言:SQL (Structured Query language)

## 實作

Rails 預設使用 SQLite3 作為資料庫，主要是在開發，測試環境上，SQLite3 是一個輕量型的資料庫類型，但到正式產品上線，SQLite3 可能會不夠用。
實作情境大致分成三種，第一種：完美 PostgreSQL 專案建立，第二種：任性手動調整 SQLite3 -> PostgreSQL ，跟最後一種：沒辦法就是要PostgreSQL

# 安裝PostgreSQL

```brew install postgresql```

使用 homebrew 安裝一鍵搞定

**啟動** PostgreSQL

```brew services start postgresql```
**關掉** PostgreSQL
```brew services stop postgresql```


# 正常一點，用PostgreSQL 建立專案

```rails new myapp --database=postgresql```

意思是：用 rails 直接 new 一個新的專案，database 使用 postgresql。 <br/>
config/database.yml檔案中可以看到 database 的設定狀態

{% highlight yml %}

default: &default
    adapter: postgresql
    encoding: unicode
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
development:
    <<: *default
    database: myapp_development
test:
    <<: *default
    database: myapp_test
production:
    <<: *default
    database: myapp_production
    username: myapp
    password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>

{% endhighlight %}

最後，在用```rails db:create```的指令讓 rails 和資料庫做連結，

![database_create](/assets/post_img/postgresql-database-create-success.png "database_create")

# 手賤一點，原本是 SQLite3 轉到 PostgreSQL

以這個情境來說，是自己建立專案，獨立開發的狀態下，

一樣， new 一個新的 Rails 專案，
```rails new myapp_default_sqlite3```

config/database.yml檔案中可以看到 database 的設定狀態

{% highlight yml %}

default: &default
  adapter: sqlite3
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: db/development.sqlite3

test:
  <<: *default
  database: db/test.sqlite3

production:
  <<: *default
  database: db/production.sqlite3

{% endhighlight %}

**來吧動手改掉他吧**

-   打開 Gemfile: 
    -   把```gem 'sqlite3' ```註解掉
    -   加上```gem 'pg' ```
    -   ```bundle```

-   打開 PostgreSQL services```brew services start postgresql```

－  進入到PostgreSQL模式，```psql```

-   直接建立資料庫```CREATE DATABASE YOUR_DATABASE_NAME```

-   修改database.yml檔


{% highlight yml %}

default: &default
  adapter: **postgresql**
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: **myapp**

test:
  <<: *default
  database: **myapp**

production:
  <<: *default
  database: **myapp**

{% endhighlight %}

-   ```rails db:create```

![database_create](/assets/post_img/sqlite3-default-switch-postgresql.png "database_create")

成功將 rails 和 PostgreSQL 的資料庫串起來了

# 第三種情境是沒有選擇，協作專案...

![database_create](/assets/post_img/clone-project.png "database_create")

database的部分，不是自己建的，要怎麼跟自己本機端的database串在一起？

這時候需要 PostgreSQL USER_NAME 跟 USER_PASSWORD ，讓這個database也可以在本機建立資料。

首先找出自己的 PostgreSQL USER_NAME 跟 USER_PASSWORD ，我的方式是透過 ```psql``` 進到資料庫，接者打指令```\du```，可以列出使用者。

這裡順便把 PostgreSQL常用語法也列出來。

|指令 |幹嘛的|
|-----|--------|
| \l | 顯示所有資料庫 |
| \du | 查看所有USER |
| \c *DatabaseName* | 切換資料庫 |
| \q | 結束 |

{% highlight yml %}
    adapter: postgresql
    encoding: unicode
    database: blog_development
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
    pool: 5
{% endhighlight %}

最後一樣，```rails db:create```，看看有沒有成功的在本地的 PostgreSQL 建立 database

![database_create](/assets/post_img/git-clone.png "database_create")

不過要是把 username / password 直接寫在檔案裡，要是進行 Git 版控裡，或是放在 GitHub 中帳號密碼會外洩，這裡有另外一個 gem 套件， [dotenv](https://github.com/bkeepers/dotenv)  

安裝步驟：

-   根據GitHub上的教學，把 dotenv 放到 Gemfile
    -   ```gem 'dotenv-rails', groups: [:development, :test]```
    -   記得```bundle```
-   目錄列建立一個```.env```檔案
    檔案放<br/>
    ```USERNAME: YOUR_USERNAME``` <br/>
    ```PASSWORD: YOUR_PASSWORD```

-   然後我們就可以直接修改檔案的內容，改成區域變數的方式

{% highlight yml %}
    adapter: postgresql
    encoding: unicode
    database: blog_development
    username: **<%= ENV['USERNAME'] %>**
    password: **<%= ENV['PASSWORD'] %>**
    pool: 5
{% endhighlight %}

在Rails中請記得區域變數一定要加上 **<%= %>** .erb檔的寫法

最後就以
**```brew services stop postgresql```**
結束這一回合!!!!!!