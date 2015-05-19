##引用recipes
###引用外部的recipes

1.add depends to `metadata.rb`

```rb
depends 'apt'
```

2.include this recipe in your recipes(i.e. `recipes/default.rb`)
```rb
include_recipe 'apt::default'
```

###引用cookbook內的recipes

include this recipe in your recipes(i.e. `recipes/default.rb`)
```rb
include_recipe 'your-cookbook::recipe-name'
```
