---
layout: post
title: What is Migration?
---

Migration (資料庫遷移)是 Active Record 的其中一項特性，讓你可以不使用 SQL 語法修改資料庫， Active Record 會紀錄哪些 migration 已經執行過、哪些還沒，並且會更新 db/schema.rb (資料庫綱要)。

在新建 model 時，可以連同欄位(title)一起新建
`rails g model Article title:string`

Rails 就會在 db 資料夾產生新的 migration 檔案，並且產出新增欄位的程式碼：

```ruby
class CreateArticles < ActiveRecord::Migration[6.0]
  def change
    create_table :articles do |t|
      t.string :title

      t.timestamps
    end
  end
end
```

確認好 migration 內容，執行 `rails db:migrate`，就會產出真實的資料表。

t.timestamps 會建立 updated_at 和 created_at 這兩個欄位， Active Record 也會在資料新增和更新時自動更新時間。

如果需要其他資料類型，下列為 Active Record 支援的資料類型：
* :binary
* :boolean
* :date
* :datetime
* :decimal
* :float
* :integer
* :primary_key
* :string
* :text
* :time
* :timestamp

### 如果想要修改 Migration？

可以使用`rails db:rollback STEP=1`，把執行過的 Migration 倒轉回去前一個步驟。

<span style="color:red">此方法通常會造成新增或修改欄位/資料表，建議直接新增一個 Migration 修改。</span>

例如想在 task 新增 user id，使用 add...to：
`rails generate migration add_user_id_to_tasks`

就會產生下列 Migration 檔案：
```ruby
class AddUserIdToTask < ActiveRecord::Migration[6.0]
  def change
    add_reference :tasks, :user, foreign_key: true
  end
end
```

執行 `rails db:migrate`後，就會在 task 新增 user id 欄位。

除了 add...to 這個方法以外，還有下列可以修改 schema 的方法：
* add_column
* add_foreign_key
* add_index
* add_reference
* add_timestamps
* change_column_default (must supply a :from and :to option)
* change_column_null
* create_join_table
* create_table
* disable_extension
* drop_join_table
* drop_table (must supply a block)
* enable_extension
* remove_column (must supply a type)
* remove_foreign_key (must supply a second table)
* remove_index
* remove_reference
* remove_timestamps
* rename_column
* rename_index
* rename_table

### 如何知道 Migration 是否執行過？

`rails db:migrate:status`可以看到 Migration 執行狀況， down 是尚未執行； up 是已經執行過的。

```ruby
database: db/rubytraining_development

 Status   Migration ID    Migration Name
--------------------------------------------------
   up     20200617051001  Create users
   up     20200617111622  Create tasks
   up     20200620133815  Add user id to task
```

參考資料：

1. [Active Record Migrations](https://guides.rubyonrails.org/active_record_migrations.html)
2. [Model Migration](https://railsbook.tw/chapters/17-model-migration.html)