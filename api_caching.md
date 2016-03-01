# API caching

Sometimes we need to cache part of our application, specially when we don't want to execute unnecesary queries to the database.

In order to add cache to your api, follow this instructions:

ActiveModelSerializers provides a method ```cache(options)``` that relays on ActiveSupport::Cache::Store,
you can send a key option that will be the prefix of the object cache on a pattern ```"#{key}/#{object.id}-#{object.updated_at}"```.


```ruby
cache(options = nil) # options: {key, expires_in, compress, force, race_condition_ttl}
```

for example:

```ruby
class ProductSerializer < ActiveModel::Serializer
  cache key: 'product', expires_in: 2.hours

  # if you don't want to specify the key
  # table_name/id-updated_at_timestamp
  # cached
  # delegate :cache_key, to: :object
  attributes :name, :price
end
```

We are telling the serializer we want to cache the result with the key product, when we update the product or in 2 hours whatever it happens first.

But sometimes we can't cache the entire endpoint, so we can use fragment cache on attributes or relationships, you can use ```only``` or ```except```

```ruby
class ShopSerializer < ActiveModel::Serializer
  cache key: 'shop', expires_in: 3.hours, only: [:title, :description]
  attributes :title, :description

  has_many :products
end
```

But what happens if one or more products change but don't expire the shop cache?

```ruby
class Product < ActiveRecord::Base
  belongs_to :shop, touch: true
end

# you may need to add this line to development.rb
config.cache_store = :memory_store
```

### For production

```ruby
# Gemfile
gem "dalli"
gem "memcachier"
...

# production.rb
config.cache_store = :dalli_store
```

