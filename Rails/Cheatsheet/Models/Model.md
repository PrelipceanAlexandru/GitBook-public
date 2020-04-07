### Models

Generate a model and create a migration for the table

**Note:** Name models in Pascal case and singular

```
$ rails g model Photo
```

Generate a model and create a migration with table columns

```
$ rails g model Photo path:string caption:text
```

The migration automatically created for the above command: 

```rb
class CreatePhotos < ActiveRecord::Migration
  def change
    create_table :photos do |t|
      t.string :path
      t.text :caption
 
      t.timestamps null: false
    end
  end
end
```

Reference: http://guides.rubyonrails.org/active_model_basics.html
