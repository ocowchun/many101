##建立FK
FK好棒棒,不用會很多問題

http://robots.thoughtbot.com/referential-integrity-with-foreign-keys?utm_source=rubyweekly&utm_medium=email

###Cascading Deletes
With a slight tweak to our foreign key, we can have the database, rather than ActiveRecord callbacks, handle the cascading deletes. Let’s change our foreign key just a bit:

```
def change
  remove_foreign_key :posts, :users
  add_foreign_key :posts, :users, dependent: :delete

  # or in the upcoming native support in Rails 4.2
  # add_foreign_key :posts, :users, on_delete: :cascade
end
```
This will run the following SQL when creating the foreign key:

```
ALTER TABLE `posts`
ADD CONSTRAINT `posts_user_id_fk`
FOREIGN KEY (`user_id`) REFERENCES `users`(id)
ON DELETE CASCADE;
```
With the dependent option functionality now moved to our foreign key, the database can now handle cleaning up the associated records. We no longer need to rely on callbacks for this behavior, so let’s remove the option.

gem 'immigrant'