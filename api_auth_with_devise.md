# API authentication (devise way)

First, devise ;)

```ruby
  # Gemfile
  gem 'devise'
```

```shell
$ rails generate devise:install
$ rails generate devise
$ rake db:migrate
```


Then, a new column for the authentication token must be added to the users table:

```shell
$ rails g migration AddAuthenticationTokenToUsers authentication_token:string
```

```ruby
class AddAuthenticationTokenToUsers < ActiveRecord::Migration
  def change
    add_column :users, :authentication_token, :string
  end
end
```

Autogenerate authentication token by the model on creation:

```ruby
class User < ActiveRecord::Base
  before_save :ensure_authentication_token

  def ensure_authentication_token
    if authentication_token.blank?
      self.authentication_token = generate_authentication_token
    end
  end

  private

    def generate_authentication_token
      loop do
        token = Devise.friendly_token
        break token unless User.where(authentication_token: token).first
      end
    end
end
```


By default, Devise’s sessions controller only responds to HTML request. It must respond to JSON.
To achieve that, define a custom sessions controller (if HTML responses are not needed the format handling can be left out of course):

```ruby
class SessionsController < Devise::SessionsController
  respond_to :html, :json

  def create
    super do |user|
      if request.format.json?
        data = {
          token: user.authentication_token,
          email: user.email
        }
        render json: data, status: 201 and return
      end
    end
  end
end
```

and configure Devise to use that controller instead of the default one:

```ruby
MyRailsApp::Application.routes.draw do
  devise_for :users, controllers: { sessions: 'sessions' }
end
```

The Rails application must authenticate users by their authentication token and email if present:

http://apidock.com/rails/v3.2.3/ActionController/HttpAuthentication/Token/ControllerMethods/authenticate_with_http_token

```ruby
class ApplicationController < ActionController::Base
  before_filter :authenticate_user_from_token!

  # Enter the normal Devise authentication path,
  # using the token authenticated user if available
  before_filter :authenticate_user!

  private

  def authenticate_user_from_token!
    authenticate_with_http_token do |token, options|
      user_email = options[:email].presence
      user = user_email && User.find_by_email(user_email)

      if user && Devise.secure_compare(user.authentication_token, token)
        sign_in user, store: false
      end
    end
  end
end
```


*The Rails application should not create session cookies*


```ruby
Rails.application.config.session_store :disabled
```

The backend app also needs to support Cross-Origin Resource Sharing, so you need to install rack-cors:

Add it to the Gemfile:

```ruby
gem 'rack-cors', :require => 'rack/cors'
```

```ruby
# config/application.rb
module YourApp
  class Application < Rails::Application

    # ...

    config.middleware.insert_before 0, "Rack::Cors" do
      allow do
        origins '*'
        resource '*', :headers => :any, :methods => [:get, :post, :options]
      end
    end

  end
end
```

```shell
$ rails c
> User.create! email: "user@example.com", password: "password"
``