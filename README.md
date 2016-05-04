# Getting Rails 5 up-and-running on c9.io (or any Ubuntu system)

A short checklist borrowed from [this blog post](http://railsapps.github.io/installrubyonrails-ubuntu.html):

## Installing Rails 5 and RVM

Simply run the following commands in order:

1. `sudo apt-get update` - updates Ubuntu packages to latest version so you don't install out-of-date versions
2. `sudo apt-get install curl` - installs the cURL utility that is used to send HTTP requests (like in the next command)
3. `\curl -L https://get.rvm.io | bash -s stable --ruby` - downloads and installs RVM (Ruby Version Manager)
4. `rvmsudo rvm get stable --autolibs=enable` - RVM updates itself to the most recent stable version
5. `rvm reload` - Reload RVM so it's using the latest version that was just installed
6. `rvm --default use ruby-2.3.0` - tells RVM to use Ruby 2.3.0 (for Rails 5)
7. `gem update --system` - updates **gem** (the Ruby package manager)
8. `gem install rails --pre` - installs the most recent stable Rails version
9. `rails -v` - checks the Rails version. It should be 5

If all of the above worked and `rails -v` is giving the correct output, you can move on.

## Creating and running a Rails app

This is a bit trickier, but nothing extremely difficult.

1. `rails new my_app_name -d postgresql` - generates a new Rails application (folders, files, etc.) with the name *my_app_name*, using PostgreSQL database
2. `sudo service postgresql start` - Start PostgreSQL so that Rails can use it
3. `rake db:create` - creates the PostgreSQL database

    Oh no, an error:
    
        PG::InvalidParameterValue: ERROR:  new encoding (UTF8) is incompatible
        with the encoding of the template database (SQL_ASCII)
        
    What a pain in the ass. Basically the template for PostgreSQL databases is encoded as SQL_ASCII, but Rails wants to create the
    database in UTF-8 encoding because of this line in `config/database.yml`:
    
    `encoding: unicode`
    
    Rails is right, PostgreSQL should have its default encoding as UTF-8. ASCII makes no sense. Whatever.
    
    To fix it, download [this script](https://gist.github.com/cireficc/1cf32a65f097e743a9c4) and execute it (you may need to run `chmod 777` on it)
    The script will simply set the default encoding for the database template used by Rails to UTF-8 isntead of SQL_ASCII.
    You should see this output:
    
        UPDATE 1
        DROP DATABASE
        CREATE DATABASE
        UPDATE 1
        VACUUM
        UPDATE 1
    
    To verify that this worked, check PostgreSQL directly:
    
    - `sudo sudo -u postgres psql` to launch the PostgreSQL console
    - `\list` to list any existing databases
    
    You should see this line: `template1 | postgres | UTF8 | C | C |`;
    Rails will use `template1` when creating databases, so as long as the encoding is UTF-8, everything will be fine.
    
    - Re-run `rake db:create`; the command should no longer throw an exception
    - Re-run `\list` in the PostgreSQL console to list databases.
    
    You should now see the database for your rails app: `my_app_name_development | ubuntu | UTF8 | C | C |`. Use `\q` to quit the console.

4. Navigate to the app's Cloud9 URL, e.g. ` https://rails-playground-cireficc.c9users.io`.
   - You should see a beautiful picture of happy people with a welcome message:

   **"Yay! You're on Rails!"**

If you got this far, then you've successfully set up Rails and are now ready to start playing around!

Here are some useful sites containing tutorials:

 - [Learn Rails on CodeCademy](https://www.codecademy.com/learn/learn-rails)
 - [Tutorials for building simple Rails apps](http://railsapps.github.io)
 - [A guy who made 12 web app clones using Rails](https://mackenziechild.me/12-in-12)