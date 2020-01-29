 use debugger with condition
 
 ```
 nano file1.rb
 ```
 
 ```ruby
require 'byebug'

10.times do |t|
   debugger if t == 5
   puts t
end
```

```
ruby file1.rb
```

