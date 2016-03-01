# API Pagination

https://github.com/amatsuda/kaminari


```ruby
gem 'kaminari'
```

```shell
$ bundle install
```

```ruby
class Api::V1::ProductsController < ApplicationController
  ...
  def index
    @products = Product.page(params[:page]).per(10)
    render json: @products
  end
  ...
end
```

The specs
```ruby
it 'Expects to be paginated' do
  FactoryGirl.create_list(:vegetable, 50)

  get api_products_path, nil, auth_header
  # parse the json response
  json = JSON.parse(response.body)

  # Remember that we set 10 records per page
  expect(json['meta']['total_pages']).to eq(5)
  expect(json['meta']['current_page']).to eq(1)
  expect(json['meta']['total_pages']).to eq(5)
  expect(json['meta']['total_count']).to eq(50)
end
```

In our api base controller
```ruby
def json_pagination(object)
  {
    current_page: object.current_page,
    next_page: object.next_page,
    prev_page: object.prev_page,
    total_pages: object.total_pages,
    total_count: object.total_count
  }
end
```

In our products controller:

```ruby
  def index
    @products = Product.page(params[:page]).per(10)
    render json: @products, meta: json_pagination(@products)
  end
```