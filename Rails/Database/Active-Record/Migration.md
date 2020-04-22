# Migrations

Migrations are a convenient way to alter your database schema over time in a consistent and easy way


## [Creating a Migration](https://guides.rubyonrails.org/active_record_migrations.html#creating-a-migration)

The base form
`rails generate migration AddPartNumberToProducts`
will generate base migration
```
class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
  def change
  end
end
```
If the migratio looks like `AddColumnToTable` or `RemoveColumnFromTable` and it is followed by a list of column names and types
it will complete with the migration detail.

`rails generate migration AddPartNumberToProducts part_number:string`

will generate
```
class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
  def change
    add_column :products, :part_number, :string
  end
end
```

## If you'd like to add an index on the new column, you can do that as well:

`rails generate migration AddPartNumberToProducts part_number:string:index`

will generate
```
class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
  def change
    add_column :products, :part_number, :string
    add_index :products, :part_number
  end
end
```

## Remove column, you can generate a migration to remove a column from the command line:

`rails generate migration RemovePartNumberFromProducts part_number:string`
will generate

```
class RemovePartNumberFromProducts < ActiveRecord::Migration[5.0]
  def change
    remove_column :products, :part_number, :string
  end
end
```

## Add References to 

`rails generate migration AddUserRefToProducts user:references`

will generate

```
class AddUserRefToProducts < ActiveRecord::Migration[5.0]
  def change
    add_reference :products, :user, foreign_key: true
  end
end
```

## Renaming a column

`rails g migration RenameProfileNameToFullName`

Will generate a base migration like this:

```
class RenameProfileNameToFullName < ActiveRecord::Migration
  def change
  end
end
```

**And you need to add:**

`rename_column :profiles, :name, :full_name`


## Changing the Column Type  Rollback is imposible without #up and #down

### Change to string

`rails g migration ChangeNameToBeStringInProfiles`

will generate a base migration like this:

```
class ChangeNameToBeStringInProfiles < ActiveRecord::Migration
  def change
  end
end
```
**And you need to add:**

`change_column :profiles, :name, :string`

### Change to integer

`rails g migration ChangeAgeToBeIntegerInProfiles`

will generate a base migration like this:

```
class ChangeAgeToBeIntegerInProfiles < ActiveRecord::Migration
  def change
  end
end
```
**And you need to add:**

`change_column :profiles, :age, :integer`

### Warning 1
1. If the column `age` was `string` and in the database exists records that can be converted to `integer`, like "20" -> 20, the migratin will be succesfull.

2. If the column `age` was `string` and in the database exists records that can't be cenverted to `integer`, like "20abc" -> error, OR ,like "ABC" -> error, the migratin will fail.

error message example:
```
Mysql2::Error: Data truncated for column 'age' at row 1
```
or 
```
Incorrect integer value: 'ABC' for column 'age' at row 1
```
The solution(my opinnion)

Write a `rake task` to check and change the records to be able to convert from string to integer

### warning 2

Note that **change_column** is an **irreversible migration**. It will cause an **error** if you try to **rollback**. To prevent this, **modify the usual change method** in the migration to use **two separate up and down methods** like this:

`rails g migration ChangeAgeToBeIntegerInProfiles`

```
class ChangeAgeToBeintegerInProfiles < ActiveRecord::Migration
  def up
    change_column :profiles, :age, :integer
  end

  def down
    change_column :profiles, :age, :string
  end
end
```

**Now the rollback will work.**




