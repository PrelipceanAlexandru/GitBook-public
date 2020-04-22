# Migrations

Migrations are a convenient way to alter your database schema over time in a consistent and easy way


## [Creating a Migration](https://guides.rubyonrails.org/active_record_migrations.html#creating-a-migration)

The base form
`rails generate migration AddPartNumberToProducts`
will generate 
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

will generate

```
class RenameProfileNameToFullName < ActiveRecord::Migration
  def change
    rename_column :profiles, :name, :full_name
  end
end
```

## Changing the Column Type






