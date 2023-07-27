#rails #authentification  [Video Guide](https://www.youtube.com/playlist?list=PLgYiyoyNPrv_yNp5Pzsx0A3gQ8-tfg66j)

## Initialization

Ensure the GemFile has `gem 'bcrypt'`
- Run `bundle install`

The [BCrypt gem](https://github.com/bcrypt-ruby/bcrypt-ruby) is open source, and their documentation has some excellent examples that demonstrate this functionality.

Generate your user resource
-  For password use `password_digest`

```ruby
create_table :users do |t|
  t.string :username
  t.string :password_digest

  t.timestamps
end
```

- In your user model insert `has_secure_password`
```ruby
class User < ApplicationRecord
  has_secure_password
end
```

The [`has_secure_password`](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html) method also provides two new instance methods on your `User` model: 
	-  password 
	- password_confirmation. 
- These methods don't correspond to database columns! Instead, to make these methods work, your `users` table **must** have a `password_digest` column.

## Setup 

- Ensure cors is enabled
Run (if rack-cors wasn't initially on the Gemfile) : 
```terminal
bundle add rack-cors
```

In `config/initializers` add `cors.rb` if it doesn't exist
- Add this to the file  :
```ruby
# Read more: https://github.com/cyu/rack-cors

Rails.application.config.middleware.insert_before 0, Rack::Cors do
    allow do
        origins 'http://localhost:5173'  # Frontend website url

        resource '*',
            headers: :any,
            methods: [:get, :post, :put, :patch, :delete, :options, :head],
            credentials: true
    end
end
```


- Create a sessions store in `config/initializers`  
	- `session_store.rb`
	- Defines how the cookies will be structured
```ruby
if Rails.env.production?
	Rails.application.config.session_store :cookie_store, key: '_random_name', domain: "BackendUrl.com"
else
    Rails.application.config.session_store :cookie_store, key: '_define_the_name'
end
```

## Validations

- Using #rails  [Active Record Validations — Ruby on Rails Guides](https://guides.rubyonrails.org/active_record_validations.html#validation-helpers)
Add validations to you user model :
```ruby
class Player < ApplicationRecord
    has_secure_password

    validates :username, :email, presence: true, uniqueness: true
    validates :password, :password_confirmation, presence: true
end
```

## Sessions

### Sign up & Login
- Create a sessions controller
```
$ rails g controller sessions
```
Insert a sessions route 
```ruby
resource :sessions
post '/login',to: 'sessions#create'
```

- Create function for creation of sessions on **Login**
```ruby
class SessionsController < ApplicationController
    def create
        player = Player.find_by(email: params[:email])
        if player&.authenticate(params[:password])
            session[:player_id] = player.id
            render json: player, status: :created
        else
            render json: { error: "Invalid email or password" }, status: :unauthorized
        end
    end
end
```

- Create a controller for handling **Sign up** -  `resource :signup`
```
$ rails g controller signups
```

In the signups controller :
```ruby
class SignupsController < ApplicationController

    def create
        player = Player.create!(valid_params)
        if player
            session[:player_id] = player.id
            render json: player, status: :created
        else
            render json: { error: "Try again after a few minutes" }, status: :internal_server_error
        end
    end
    
    private

    def valid_params
        params.permit(:email, :password, :password_confirmation, :username)
    end
end
```

### Logged in & Log out

To check whether user is logged in or to let a user log out.
In routes add : 
```ruby
delete '/logout', to: 'sessions#destroy'
get '/logged_in', to: 'sessions#logged_in'
```

Create a concern for the current user in `controllers/concerns`  create `current_user_concern.rb`
- Insert :
```ruby
module CurrentUserConcern
    extend ActiveSupport::Concern

    included do
        before_action :set_current_user
    end

    def set_current_user
        if session[:player_id] # change according to how you wrote it on session create
            @current_user = Player.find(session[:player_id])
        end
    end
end
```

In sessions controller :

``` ruby
class SessionsController < ApplicationController
    include CurrentUserConcern

    def create
        player = Player.find_by(email: params[:email])
        if player&.authenticate(params[:password])
            session[:player_id] = player.id
            render json: player, status: :created
        else
            render json: { error: "Invalid email or password" }, status: :unauthorized
        end
    end  

    def destroy
        reset_session
        render json: { message: "Successfully logged out" }, status: :ok
    end

    def logged_in
        if @current_user
            render json: @current_user, status: :ok
        else
            render json: { logged_in: false }
        end
    end
end
```
'../React/Login&Auth'
Continue on the [Frontend_setup](Login-signup-logout.md) 



