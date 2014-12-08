```sh
rails generate rspec:install
```

add 'spec/support/factory_girl.rb'
```rb
RSpec.configure do |config|
  config.include FactoryGirl::Syntax::Methods
end
```