#rails 
Do the creation process on  [new rails app](Rails/Rails-configuration)

Guide -> [GitHub](https://github.com/edd-ie/rails-deploment-guide)

Configure your project to run in a production environment with *Render*
```terminal
$ bundle lock --add-platform x86_64-linux
```

### Enable Cors

Run add : 
```terminal
bundle add rack-cors
```

In `config/initializers` add `cors.rb` if it doesn't exist
- Add this to the file  :
```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    
    resource '*',
            headers: :any,
            methods: [:get, :post, :put, :patch, :delete, :options, :head]\
	
	resource '/orders', # for specific restrictions(optional)
      headers:any,
      methods: [:post]
  end
end
```

## Preparations

- Open the `config/database.yml` file, scroll down to the `production` section, and update the code to the following:

```ruby
production:
  <<: *default
  url: <%= ENV['DATABASE_URL'] %>
```

- Open `config/puma.rb` and find the section shown below. Here, you will un-comment out two lines of code and make one small edit:
```ruby
#
workers ENV.fetch("WEB_CONCURRENCY") { 4 } ### CHANGE: Un-comment out this line; update the value to 4

# process behavior so workers use less memory.
#
preload_app! ### CHANGE: Un-comment out this line
```

Next, open the `config/environments/production.rb` file and find the following line:

```ruby
# `config/environments/production.rb`(like line:24)
config.public_file_server.enabled = ENV["RAILS_SERVE_STATIC_FILES"].present?
```

Update it to the following:

```ruby
config.public_file_server.enabled = ENV['RAILS_SERVE_STATIC_FILES'].present? || ENV['RENDER'].present?
```

Finally, inside the `bin` folder create a `render-build.sh` script and copy the following into it:
```sh
#!/usr/bin/env bash
# exit on error
set -o errexit

# Build commands for back end
bundle install
bundle exec rake db:migrate 
bundle exec rake db:seed # if you have seed data, run this command for the initial deploy only to avoid duplicate records
```

 On the vscode terminal change from PowerShell/cmd to git bash, _This will enable running Linux code_ , run:
```sh
chmod a+x bin/render-build.sh
```

### Creating the Render Web Service
1. Push app to GitHub
2. Ensure you created a PostgreSQL server on render first
3. Click **New +** button and select _Web Service_
4. Select your repo from the list
5. On the site edit the following:
```
Name : whatever you want
Runtime: ruby
Build command: ./bin/render-build.sh
Start Command: bundle exec puma -C config/puma.rb
```
6. On different tab open the render psql server (step 2):
	1. Click **connect**
	2. Select **Internal Database URL**
	3. Copy the url
7. Back at the web service settings, scroll down and click advanced:
8. Add environment variables(2)
```
DATABASE_URL : paste the postgres url
RAILS_MASTER_KEY: copy from 'config/master.key'
```

9. Complete page should look like: 
![[deploy_review.png]]

10. Click **Create Web Service**. The deploy process will begin automatically.

NOTE: if errors ok they are easily fixable by reading the message on the terminal.

**IMPORTANT !** : on `bin/render-build.sh` comment out db:migrate and db:seed lines after creation of tables and insertion of dummy data
 - To avoid duplicate data
 - Faster deployment speed