- Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic

- [Active Record it is a Pattern](https://www.martinfowler.com/eaaCatalog/activeRecord.html)

- Object Relational Mapping - is a technique that connects the rich objects of an application to tables in a relational database management system

Naming Conventions
 - Model Class - Singular with the first letter of each word capitalized (e.g., BookClub).
 - Database Table - Plural with underscores separating words (e.g., book_clubs).
 

|Model Class| Table Schema |
|---     |---              |
|Article | articles        |
|LineItem| line_items      |
|Deer    | deers           |
|Mouse   | mice            |
|Person  | people          |

[Schema Conventions](https://guides.rubyonrails.org/active_record_basics.html#schema-conventions)

Overriding the Naming Conventions
 You can use the ActiveRecord::Base.table_name= method to specify the table name that should be used:
 ```
class Product < ApplicationRecord
 self.table_name = "my_products"
end
```
It's also possible to override the column that should be used as the table's primary key using the ActiveRecord::Base.primary_key= method:
```
class Product < ApplicationRecord
  self.primary_key = "product_id"
end
```

[Rails Migration](https://guides.rubyonrails.org/active_record_migrations.html) 

extract what is most used from above link Rails Migration

