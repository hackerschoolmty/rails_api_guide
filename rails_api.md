###Versioning

Versioning helps prevent major changes from breaking existing clients.

We can version the API and allow the access through different strategies:

**URI param**

With this approach you will expect a URI like:

`http://hackerschool.com/api/:version`

Here the `:version` could be:

* v1
* 1
* 1.023243
* v2.1.3

It really depends on your versioning structure.

**URL param**

With this approach you will expect a URI like:

`http://hackerschool.com/api?version=:version`

Here the `:version` could be:

* v1
* 1
* 1.023243
* v2.1.3

It really depends on your versioning structure.

**Request header**

With this approach you will expect a URI like:

`http://hackerschool.com/api/`

But on the request headers, you have to include something like:

`Accept: application/vnd.hackerschool.mx+json; version=1`

Here the `:version` could be:

* v1
* 1
* 1.023243
* v2.1.3

It really depends on your versioning structure.

--

*It is highly recommended that you keep your api behind a subdomain, this way you can scalate via DNS, in case your API is being under a heavy demand*


## Working with Rails APIs

```
$ rails new my_api
#       -or-
$ rails new my_api --api
```

### ActiveModelSerializers

> ActiveModel::Serializers brings convention over configuration to your JSON generation.
>
> https://github.com/rails-api/active_model_serializers

###User handling

In this course we will keep things simple and we won't use [Devise](https://github.com/plataformatec/devise) to handle users authentication, instead we will use the built-in authentication module that comes with Rails.

We need a `User` model with the following attributes:

```console
% rails g model User email password_digest
% rake db:migrate
```

After that we just need to update our `User` model to include a class method named `has_secure_password`

```ruby
class User < ActiveRecord::Base
  has_secure_password
end
```

We still need to add some simple validations to our user model:

```ruby
class User < ActiveRecord::Base
  has_secure_password

  validates :email, presence: true,
                  uniqueness: { :case_sensitive => false }

  # We add the confirmation validation, as Rails won't do it for us =(
  validates :password,
    :length => { :minimum => 8 },
    :confirmation => true
end
```

We can now authenticate our users like:

```
user = User.new(name: 'david', password: '', password_confirmation: 'nomatch')
user.save                                                       # => false, password required
user.password = 'mUc3m00RsqyRe'
user.save                                                       # => false, confirmation doesn't match
user.password_confirmation = 'mUc3m00RsqyRe'
user.save                                                       # => true
user.authenticate('notright')                                   # => false
user.authenticate('mUc3m00RsqyRe')                              # => user
User.find_by(name: 'david').try(:authenticate, 'notright')      # => false
User.find_by(name: 'david').try(:authenticate, 'mUc3m00RsqyRe') # => user
```

Once we have our validations on the model, we can jump into our controller and set some actions to handle `authentication`.
