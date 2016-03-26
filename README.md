# Installation of RVM and Rails 5

A short cheklist borrowed from [this blog post](http://railsapps.github.io/installrubyonrails-ubuntu.html):

- `sudo apt-get update`
  - Updates Ubuntu packages to latest version so you don't install out-of-date versions
- `sudo apt-get install curl`
  - Installs the cURL utility that is used to send HTTP requests (like in the next command)
- `\curl -L https://get.rvm.io | bash -s stable --ruby`
  - Downloads and installs RVM (Ruby Version Manager)
- `rvmsudo rvm get stable --autolibs=enable`
  - RVM updates itself to the most recent stable version
- `rvm --default use ruby-2.3.0`
  - Tells RVM to use Ruby 2.3.0 (for Rails 5)
- `gem update --system`
  - Updates **gem** (the Ruby package manager)
- `gem install rails --pre`
  - Installs the most recent stable Rails version
- `rails -v`
  - Checks the Rails version. It should be 5
- `rails new my_app_name -d postgresql`
  - Generates a new Rails application (folders, files, etc.) with the name *my_app_name*, using PostgreSQL database