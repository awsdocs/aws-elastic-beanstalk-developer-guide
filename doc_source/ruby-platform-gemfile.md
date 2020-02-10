# Installing packages with a Gemfile<a name="ruby-platform-gemfile"></a>

Use a `Gemfile` file in the root of your project source to use RubyGems to install packages that your application requires\.

**Example Gemfile**  

```
source "https://rubygems.org"
gem 'sinatra'
gem 'json'
gem 'rack-parser'
```

When a `Gemfile` file is present, Elastic Beanstalk runs `bundle install` to install dependencies\.