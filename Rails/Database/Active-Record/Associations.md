[Active record associations](https://guides.rubyonrails.org/association_basics.html)

# The Types of Associations

- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many

### The belongs_to Association
[belongs_to](https://guides.rubyonrails.org/association_basics.html#the-belongs-to-association)

[commit cc2406f5722c7cb615976f4572ef6fa0354d02e1](https://github.com/PrelipceanAlexandru/lite_red/commit/cc2406f5722c7cb615976f4572ef6fa0354d02e1)

A belongs_to association sets up a one-to-one connection with another model, such that each instance of the declaring model "belongs to" one instance of the other model

The migration looks like:
`rails g model Author name`

`db/migrate/20200423141445_create_authors.rb`
```
class CreateAuthors < ActiveRecord::Migration[6.0]
  def change
    create_table :authors do |t|
      t.string :name

      t.timestamps
    end
  end
end
```
`rails g model Book name total_pages:integer author:references`

`db/migrate/20200423141522_create_books.rb`

```
class CreateBooks < ActiveRecord::Migration[6.0]
  def change
    create_table :books do |t|
      t.string :name
      t.integer :total_pages
      t.references :author, null: false, foreign_key: true

      t.timestamps
    end
  end
end
```

`app/models/book.rb`

```
class Book < ApplicationRecord
  belongs_to :author
end
```
**This is a bi-directional association**
```
author = Author.create(name: "Author 1")
book_1 = Book.create(name: "Book 1", total_pages: 11, author: author)
book_2 = Book.create(name: "Book 2", total_pages: 12, author: author)

query:

book_1.author
=> #<Author id: 1, name: "Author 1", created_at: "", updated_at: "">

author.first.books
=> NoMethodError (undefined method `books' for #<Author:>)
```
### The has_one Association
[has_one](https://guides.rubyonrails.org/association_basics.html#the-has-one-association)

A has_one association also sets up a one-to-one connection with another model, but with somewhat different semantics (and consequences).






