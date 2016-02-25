# Namespaces

Specify the name of the namespace we will create
```ruby
# routes.rb
  # ...
  namespace :admin do
    resources :articles
  end
```

```ruby
class ArticlesController < ApplicationController
  #...
end
```

If you want to control more actions in the namespace you should create a controller with the same name

```ruby
#controllers/admin_controller.rb
class AdminController < ApplicationController
  #...
  before_action :ensure_admin_presence

  def ensure_admin_presence
    redirect_to root_path and return if user.role != "admin"
  end
end
```

So you can use it like this:

```ruby
#controllers/admin/articles_controller.rb
class Admin::ArticlesController < AdminController
  #...
end
```