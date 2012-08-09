# Artfully Open Source Edition

An Open-source application to run your arts organization.  Features include:

* Free and paid ticketing on the web
* Patron tracking
* Patron importing and exporting
* Order tracking
* Door lists
* Box office

And coming soon...

* Advanced patron searching
* Mailchimp integration

# About

Creating your own Artfully installation requires a basic understanding of Git, Heroku, Ruby and Rails.

## Dependencies

You'll need the following apps/services to use Artfully

* Solr
* MySQL
* Braintree
* S3

## Prerequisites

Make sure you have the following installed on your system

* [Ruby](http://www.ruby-lang.org)
* [Rubygems](http://docs.rubygems.org/read/chapter/3/)
* [Rails 3.2](http://rubyonrails.org/)
* [Heroku cli](https://github.com/heroku/heroku/)
* [Git](http://git-scm.com/)

## Clone the app 

    git clone git@github.com:fracturedatlas/artfully_app.git
    cd artfully_app

## Database

Update database.yml to point to your mysql database.  Specify a local database and a production database.

Run the migrations

    bundle exec rake artfully_ose_engine:install:migrations
    bundle exec rake db:migrate

## Running Locally

If you intend to do any custom development or testing, go ahead and set up Artfully locally on your machine.  If you have no interest in this, feel free to skip ahead to Deployment to Heroku

### Setup

    gem install foreman
    bundle install
    
Open a rails console and run

    User.create!({:email => "youradmin@example.com", :password => "your_strong_password", :password_confirmation => "your_strong_password" })
    
### Run

    foreman start
    
Open `http://localhost:5000` in a browser

### Environment

Artfully requires the following environment variables to be set if they aren't explicitly set in `config/environment.rb`

    BRAINTREE_MERCHANT_ID
    BRAINTREE_PUBLIC_KEY
    BRAINTREE_PRIVATE_KEY

    S3_BUCKET
    S3_ACCESS_KEY_ID
    S3_SECRET_ACCESS_KEY

## Deployment to Heroku

### About delayed_job

Artfully ships with `delayed_job` disabled.  If you do have a Heroku worker turned on, you'll want to enable delayed_jobs.

To enable `delayed_job`, in `config/application.rb` change this line to read

    Delayed::Worker.delay_jobs = true
    
__Please note__ that Artfully depends on delayed jobs for locking tickets while a patron is checking out.  Leaving delayed jobs disabled prevents tickets from being locked.

### Push 

Follow the [Heroku instructions](https://devcenter.heroku.com/articles/creating-apps) for creating an app

    heroku apps:create myapp
    git remote add heroku git@heroku.com:myapp.git
    git push heroku master

### Setup the production database

    heroku run bundle exec rake db:migrate

### Set environment variables

If you didn't set environment variables in `config/environment.rb`, you'll need to set them on the heroku command line.  Run this command for each environment variable listed in "Environment"
    
    heroku config:add BRAINTREE_MERCHANT_ID='...'
    
If you have a Google Analytics account, you can set the environment variables `GA_ACCOUNT` and `GA_DOMAIN` to enable Google Analytics in Artfully.

### Create your first user

This user will be the organization administrator.  In a Heroku console, run

    User.create!({:email => "youradmin@example.com", :password => "your_strong_password", :password_confirmation => "your_strong_password" })

### Restart the app

    heroku restart
  
### Load your application

Go to `http://myapp.herokuapp.com` in a web browser