# Active Record

## Ruby Associations:

Output for different Ruby generator:

**Create a new model**

```ruby
rails g model Book name:string page_number:integer
```

Will generate:

```ruby
class CreateBooks < ActiveRecord::Migration[6.0]
  def change
    create_table :books do |t|
      t.string :name
      t.integer :page_number
      t.timestamps
    end
  end
end
```

**Add references to a table**. We have two way of doing this.

```ruby
rails g migration AddAuthorToBook
```

Will generate:

```ruby
class AddAuthorToBook < ActiveRecord::Migration[6.0]
  def change
  end
end
```

And you need to add manualy the references

```ruby
...
  def change
    add_reference :books, :author, foreign_key: true
  end
...
```

**OR**

```ruby
rails g migration AddAuthorToBook book:references
```

Will generate:

```ruby
class AddAuthorToBook < ActiveRecord::Migration[6.0]
  def change
    add_reference :books, :author, null: false, foreign_key: true
  end
end
```
**This problem appear for sqlite**
The problem that I encounted with the second migration is that because we have **null: false** when I run **rails db:migrate** I have the following error:

```ruby
SQLite3::SQLException: Cannot add a NOT NULL column with default value NULL
```

Removing the **null: flase** will solve the problem.

**For postgresql this problem does not appear**

**Add a column to a table**

```ruby
rails g migration AddAgeToUser age:integer
```

Will generate:

```ruby
class AddAgeToUser < ActiveRecord::Migration[6.0]
  def change
    add_column :users, :age, :integer
  end
end
```

**create Join Table using another model**

Let's say that we have a User and a Role table

```ruby
rails g model UserRole user:references role:references
```

Will generate

```ruby
class CreateUserRoles < ActiveRecord::Migration[6.0]
  def change
    create_table :user_roles do |t|
      t.references :user, null: false, foreign_key: true
      t.references :role, null: false, foreign_key: true
      t.timestamps
    end
  end
end
```

**create Join Table using direct connection. Without another model**

Let's say that we have a Note and a User model.

```ruby
rails g migration  CreateJoinTableNotesUsers notes users
```

Will generate

```ruby
class CreateJoinTableNotesUsers < ActiveRecord::Migration[6.0]
  def change
    create_join_table :notes, :users do |t|
      # t.index [:note_id, :user_id]
      # t.index [:user_id, :note_id]
    end
  end
end
```

#### 1. belongs\_to association: set for one-to-one connection with another model.

A **book** has one **author**

```ruby
rails g model Book name:string page_number:integer
rails g model Author name:string
```

```ruby
rails g migration AddAuthorToBook
```

In models/book.rb:

```ruby
class Book < ApplicationRecord
  belongs_to :author
end
```

Create new record

```ruby
Author.create(name: "Author_2")
Book.create(name: "Book_3", page_number: 6, author_id: 2)
book = Book.find_by(name: "Book_3")
author = Author.find_by(name: "Author_2")
```

Access the author for **book**

```ruby
book.author
```

The response

```ruby
#<Author id: 2, name: "Author_2", created_at: "2019-11-22 15:44:12", updated_at: "2019-11-22 15:44:12">
```

#### 2. has\_many association: set for one-to-many connection with another model.

An **author** has many **books**

In /moldes/author.rb

```ruby
class Author < ApplicationRecord
  has_many :books
end
```

```ruby
Author.create(name: "Author_10")
Book.create(name: "Book_10", page_number: 6, author_id: 3)
Book.create(name: "Book_11", page_number: 2, author_id: 3)
Author.last.books
#<ActiveRecord::Associations::CollectionProxy [
#<Book id: 5, name: "Book_3", page_number: 6, created_at: "2019-11-22 15:44:22", updated_at: "2019-11-25 08:31:47", author_id: 3>, 
#<Book id: 6, name: "Book_11", page_number: 2, created_at: "2019-11-25 08:33:38", updated_at: "2019-11-25 08:33:38", author_id: 3>
]> 
Book.last.author

#<Author id: 3, name: "Author_10", created_at: "2019-11-25 08:31:31", updated_at: "2019-11-25 08:31:31">
```

#### 3. has\_many :through association: is often used to set up a many-to-many connection with another model.

Create **User** and **Role** model. Create **UserRole** model that represents the model used for this association.

```ruby
rails g model User name:string
rails g model Role name:string
```

```text
rails g model UserRole user:references role:references
```

In /models/role.rb

```ruby
class Role < ApplicationRecord
  has_many :user_roles
  has_many :users, through: :user_roles
end
```

In /models/user.rb

```ruby
class User < ApplicationRecord
  has_many :user_roles
  has_many :roles, through: :user_roles
end
```

In /models/user\_role.rb

```ruby
class UserRole < ApplicationRecord
  belongs_to :user
  belongs_to :role
end
```

Create some records.

```ruby
User.create(name: "User_1")
User.create(name: "User_2")
Role.create(name: "Role_1")
Role.create(name: "Role_2")
```

Add **2 roles** for **first user**

```ruby
UserRole.create(user: User.first, role: Role.first)
UserRole.create(user: User.first, role: Role.second)
```

When call:

```ruby
User.first.roles
```

The response is

```ruby
#<ActiveRecord::Associations::CollectionProxy[
#<Role id: 1, name: "Role_1", created_at: "2019-11-25 08:58:39", updated_at: "2019-11-25 08:58:39">, 
#<Role id: 2, name: "Role_2", created_at: "2019-11-25 08:58:44", updated_at: "2019-11-25 08:58:44">
]>
```

When call:

```ruby
Role.first.users
```

The response is:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<User id: 1, name: "User_1", created_at: "2019-11-25 08:58:30", updated_at: "2019-11-25 08:58:30">
]>
```

When call:

```ruby
Role.second.users
```

The response is:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<User id: 1, name: "User_1", created_at: "2019-11-25 08:58:30", updated_at: "2019-11-25 08:58:30">
]>
```

Add **2 users** for **first role**

_!The first user already has first role_

```ruby
UserRole.create(user: User.last, role: Role.first)
```

When call:

```ruby
Role.first.users
```

The response is:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<User id: 1, name: "User_1", created_at: "2019-11-25 08:58:30", updated_at: "2019-11-25 08:58:30">, 
#<User id: 2, name: "User_2", created_at: "2019-11-25 08:58:42", updated_at: "2019-11-25 08:58:42">
]>
```

### This migration is **not unique**, we can add the same **User** and **Role** to **UserRole** model.

```ruby
UserRole.create(user: User.first, role: Role.first)
UserRole.create(user: User.first, role: Role.second)
UserRole.create(user: User.first, role: Role.first)
```

When call:

```ruby
User.first.roles
```

The response id:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<Role id: 1, name: "Role_1", created_at: "2019-11-25 08:58:39", updated_at: "2019-11-25 08:58:39">, 
#<Role id: 2, name: "Role_2", created_at: "2019-11-25 08:58:44", updated_at: "2019-11-25 08:58:44">, 
#<Role id: 1, name: "Role_1", created_at: "2019-11-25 08:58:39", updated_at: "2019-11-25 08:58:39">
]>
```

If you want that the record from **UserRole** to be unique you need to add the following in **UserRole**

In /models/user\_role.rb

```ruby
class UserRole < ApplicationRecord
  ...

  validates_uniqueness_of :user, scope: :role
end
```

When call:

```ruby
UserRole.create(user: User.first, role: Role.first)
UserRole.create(user: User.first, role: Role.second)
```

It is ok, but when we try to add again first user and first role we will receive an error

```ruby
UserRole.create(user: User.first, role: Role.first)
```

We will receive:

```ruby
rollback transaction
```

_!we can see the error by calling **errors**_

```ruby
UserRole.create(user: User.first, role: Role.first).errors
```

The response is

```ruby
#<ActiveModel::Errors:0x00005583e5d99d88 
@base=#<UserRole id: nil, user_id: 1, role_id: 1, created_at: nil, updated_at: nil>, 
@messages={:user=>["has already been taken"]}, 
@details={:user=>[{
  :error=>:taken, 
  :value=>#<User id: 1, name: "User_1", created_at: "2019-11-25 08:58:30", updated_at: "2019-11-25 08:58:30">}
]}>
```

Same goes for **role**

#### 4. A has\_and\_belongs\_to\_many association creates a direct many-to-many connection with another model, with no intervening model.

Create another model **notes**

```ruby
rails g model Note name:string
```

Create Join Table

#### !The tables should be alphabetical order

```ruby
rails g migration  CreateJoinTableNotesUsers notes users
```

Add the following in **User** and **Note** model class.

```ruby
class Note < ApplicationRecord
  has_and_belongs_to_many :users
end
```

```ruby
class User < ApplicationRecord
  ...

  has_and_belongs_to_many :notes
end
```

Create 3 notes record:

```ruby
Note.create(name: "Note_1")
Note.create(name: "Note_2")
Note.create(name: "Note_3")
```

Let's select the first user

```ruby
us = User.first
```

To add notes to user we need to user the **&lt;&lt;** operator or we can use **push** operator on the **user** colection

```ruby
us.notes << Note.first
us.notes.push(Note.second)
```

To display all user's notes we call:

```ruby
us.notes
```

The output will be:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<Note id: 1, name: "Note_1", created_at: "2019-11-25 15:23:25", updated_at: "2019-11-25 15:23:25">, 
#<Note id: 2, name: "Note_2", created_at: "2019-11-25 15:26:42", updated_at: "2019-11-25 15:26:42">
]>
```

For notes:

```ruby
Note.first.users.push(User.second)
```

The output\(The first user was added above\)

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<User id: 1, name: "User_1", created_at: "2019-11-25 08:58:30", updated_at: "2019-11-25 08:58:30">, 
#<User id: 2, name: "User_2", created_at: "2019-11-25 08:58:42", updated_at: "2019-11-25 08:58:42">]>
```

To delete one note, we can use **delete**

```ruby
us.notes.delete(Note.first)
```

To delete all notes, we can use **clear**

```ruby
us.notes.clear
```

This migration is not **unique**

### Let's add uniqer to note and user

First:

Create a new migration that add uniq

```ruby
rails g migration add_uniq_to_join_table
```

The migration file look like this.

```ruby
class AddUniqToJoinTable < ActiveRecord::Migration[6.0]
  def change
    add_index :notes_users, [ :note_id, :user_id ], unique: :true
  end
end
```

This can be done when we create the join table migration.

```ruby
class CreateJoinTableNotesUsers < ActiveRecord::Migration[6.0]
  def change
    create_join_table :notes, :users do |t|
      # t.index [:note_id, :user_id]
      # t.index [:user_id, :note_id]
    end
    add_index :notes_users, [ :note_id, :user_id ], unique: :true # (1)
  end
end
```

Add **add\_index** \(1\), be carefull to be autside the **create\_join\_table** block.

Now add notes to user.

```ruby
User.first.notes << Note.first
```

The insertion is succsessfuly executed.

Let's try to add again the same note to the same user

```ruby
User.first.notes << Note.first
```

The insertion will fall. The error is:

```ruby
ActiveRecord::RecordNotUnique (SQLite3::ConstraintException: UNIQUE constraint failed: notes_users.note_id, notes_users.user_id)
```

If we try to add first user to first note, the revers operation, kind of.

```ruby
Note.first.users << User.first
```

We will reveihe the same error:

```ruby
ActiveRecord::RecordNotUnique (SQLite3::ConstraintException: UNIQUE constraint failed: notes_users.note_id, notes_users.user_id)
```

## Polymorphic Associations:

[https://culttt.com/2016/01/13/creating-polymorphic-relationships-in-ruby-on-rails/](https://culttt.com/2016/01/13/creating-polymorphic-relationships-in-ruby-on-rails/)

A model can belong to more than one other model, on a single association.  
We have 3 model **Project, Task, and the Attachment**. All can be _commentable_. Let's create the **Coment** model

```ruby
rails g model Coment body:text commentable:references{polymorphic}
```

Will generate

```ruby
class CreateComments < ActiveRecord::Migration[6.0]
  def change
    create_table :comments do |t|
      t.text :body
      t.references :commentable, polymorphic: true, null: false
      t.timestamps
    end
  end
end
```

The **references** automatically creates **commentable\_type** and **commentable\_id**

Add this in **Comment** model

```ruby
class Comment < ActiveRecord::Base  
  belongs_to :commentable, polymorphic: true  
end
```

Next creates the models

```ruby
rails g model Project name:string
rails g model Task name:string
rails g model Attachments name:string
```

In each model add:

```ruby
class Project < ActiveRecord::Base  
  has_many :comments, as: :commentable  
end
class Task < ActiveRecord::Base  
  has_many :comments, as: :commentable  
end
class Attachments < ActiveRecord::Base  
  has_many :comments, as: :commentable  
end
```

Create a project

```ruby
Project.create(name: "pr1")
```

Add a comment to first project

```ruby
project = Project.first
project.comments << Comment.new(body: "Comment project 1")
```

Display all comments for project

```ruby
project.comments
```

Will display:

```ruby
#<ActiveRecord::Associations::CollectionProxy [
#<Comment id: 1, body: "Hello pr1", commentable_type: "Project", commentable_id: 1, created_at: "2019-12-03 12:48:55", updated_at: "2019-12-03 12:48:55">]>
```

Also

```ruby
Comment.first.commentable
```

Will display the first project

```ruby
#<Project id: 1, name: "pr1", created_at: "2019-12-03 12:48:20", updated_at: "2019-12-03 12:48:20">
```

And

```ruby
Comment.first
```

```ruby
#<Comment id: 1, body: "Hello pr1", commentable_type: "Project", commentable_id: 1, created_at: "2019-12-03 12:48:55", updated_at: "2019-12-03 12:48:55">
```

For Task

```ruby
Task.create(name: "first task")
```

```ruby
task = Task.first
task.comments << Comment.new(body: "task c")
```

And the comment record

```text
Comment.last
```

Will display:

```ruby
#<Comment id: 4, body: "task c", commentable_type: "Task", commentable_id: 1, created_at: "2019-12-03 12:57:36", updated_at: "2019-12-03 12:57:36">
```

## Display columns

```ruby
User
```

```ruby
User(id: integer, name: string, created_at: datetime, updated_at: datetime, age: integer)
```

## Check rails Constraint

```ruby
User.columns
```

```ruby
 [#<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4dbd00 @name="id", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f5054011458 @sql_type="integer", @type=:integer, @limit=nil, @precision=nil, @scale=nil>, @null=false, @default=nil, @default_function=nil, @collation=nil, @comment=nil>, #<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4dad38 @name="name", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f505c4dae50 @sql_type="varchar", @type=:string, @limit=nil, @precision=nil, @scale=nil>, @null=true, @default=nil, @default_function=nil, @collation=nil, @comment=nil>, #<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4d9730 @name="created_at", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f505c4d9870 @sql_type="datetime(6)", @type=:datetime, @limit=nil, @precision=6, @scale=nil>, @null=false, @default=nil, @default_function=nil, @collation=nil, @comment=nil>, #<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4d93e8 @name="updated_at", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f505c4d9460 @sql_type="datetime(6)", @type=:datetime, @limit=nil, @precision=6, @scale=nil>, @null=false, @default=nil, @default_function=nil, @collation=nil, @comment=nil>, #<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4d90a0 @name="age", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f505c4d9168 @sql_type="integer", @type=:integer, @limit=nil, @precision=nil, @scale=nil>, @null=true, @default=nil, @default_function=nil, @collation=nil, @comment=nil>]
```

```ruby
User.columns.first
```

```ruby
 #<ActiveRecord::ConnectionAdapters::Column:0x00007f505c4dbd00 @name="id", @sql_type_metadata=#<ActiveRecord::ConnectionAdapters::SqlTypeMetadata:0x00007f5054011458 @sql_type="integer", @type=:integer, @limit=nil, @precision=nil, @scale=nil>, @null=false, @default=nil, @default_function=nil, @collation=nil, @comment=nil>
```

### Update rows examples

## 3. has\_many :through association:

Add name column on UserRole UserRole.find\_by\(user: User.first, role: Role.first\).update\(name: "new name"\)

If we try to update again this record with the same data Rails will return true and will not update the record.


  self.inheritance_column = 'sti_type'
  https://stackoverflow.com/questions/11984893/issue-with-column-name-type-in-rails-3

