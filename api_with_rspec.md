# RSPEC

### Setup

```ruby
# Gemfile

gem 'active_model_serializers'

group :development, :test do
  gem "factory_girl_rails"
  gem 'ffaker'
end

group :test do
  gem "rspec-rails", "~> 2.14"
  gem "shoulda-matchers"
end

```

You could use [Platter](https://github.com/IcaliaLabs/platter).
platter is a Rails app generator gem crafted in Icalia labs.

It will create the structure of a rails application and added the necessary gems and stuff for apis.