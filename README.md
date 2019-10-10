# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

rails new api_test --api
bundle install
rake db:create

Add to Gemfile
# inside test / development
gem 'rspec-rails'
gem 'factory_girl_rails'
# all
gem 'active_model_serializers'
bundle install

rails g rspec:install

# Tell our serializer to use json:api standard
# https://jsonapi.org/
touch config/initializers/active_model_serializers.rb
ActiveModelSerializers.config.adapter = ActiveModelSerializers::Adapter::JsonApi

# User cUrl to make requests
curl -X GET -H "Accation/json" "localhost:3000/api/v1/users"

# CORS
gem 'rack-cors'
# https://github.com/cyu/rack-cors
bundle install
# Edit config/application.rb allow (origin, resource, method)
config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    resource '*', headers: :any, methods: [:get, :post, :options]
  end
end
# Test CORS on localhost using chrome
# $.ajax("http://localhost:3000/api/v1/users")

# Rate Limiting / Throttling
gem 'rack-attack'
# https://github.com/kickstarter/rack-attack
bundle install
# Edit config/application.rb to add Rack Attack middleware
config.middleware.use Rack::Attack
touch config/initializers/rack_attack.rb
# Add rules for whitelisting localhost, throttling to limit # of requests per x seconds, and logging requests
# Try adding a throttling rule and then test requests to break it and see the result

rails g model user name email
rails g serializer user
rails g scaffold_controller api::v1::users --model-name=user

