#rails  #ruby 
Create an API:
```terminal
rails new example-project -T -d=postgresql --skip-javascript

	'-T' -->  skips creation of test files
	'--api' --> only backend
	'-d=postgresql' --> configures PostgreSQL as the database 
	'--skip-javascript' --> doestn't create an node packages.
```
Once created go to:
```ruby
database.yaml

Go to development  -- if you are using postgres as development db 

Uncomment out:
	- username 
	- password
	- host
 
 Assign value 
	 username: postgres
	 password: overlord
	 host: localhost
	 port: 5432
```

Add the [ActiveModelSerializers](https://github.com/rails-api/active_model_serializers) gem:
```terminal
bundle add active_model_serializers

bundle add rack-cors
```

 Un-comment the `bcrypt` gem from your **Gemfile**, and run `bundle install`


#### Cookies & Sessions
Add session and cookie support back in, update your application's configuration in the `config/application.rb` file:

```ruby
module AppNAMe
  class Application < Rails::Application
    # ▾ Must add these lines! ▾
    # Adding back cookies and session middleware
    config.middleware.use ActionDispatch::Cookies
    config.middleware.use ActionDispatch::Session::CookieStore

    # Use SameSite=Strict for all cookies to help protect against CSRF
    config.action_dispatch.cookies_same_site_protection = :strict
  end
end
```

**Note**: _The last line also adds some additional security to your cookies by configuring the `SameSite` policy for your cookies as `strict`, which means that the browser will only send these cookies in requests to websites that are on the same domain. This is a relatively new feature, but an important one for security! You can read more about [`SameSite` cookies here](https://web.dev/samesite-cookies-explained/)._

To access the `cookies` hash in your controllers, you also need to include the `ActionController::Cookies` module in your `ApplicationController`:
```ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::API
  include ActionController::Cookies
end
```


#### Add fall back
- This is used for when the app is as the age the user is redirected if the is no data to be displayed or wrong routes is inputted
```ruby
# In routes.rb add:
  get "*path", to: "fallback#index", constraints: ->(req) { !req.xhr? && req.format.html? }

# Generate and open the fall back controller
$ rails g controller fallback

# In the fallback add
    def index

        render file: 'public/404.html'

    end

```


#### Create your models and migrations:
```terminal
rails db:create
	- Check on the terminal ignore the wordy stuff just look for:
		**Created database 'nameOfDB_development'**
	- You can check on *PgAdmin* or *psql cmd* if it was created

$ rails s

- The server starts
	Create your migrations

$ rails g resource name ....
```

