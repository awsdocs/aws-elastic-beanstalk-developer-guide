# Configuring the application process with a Procfile<a name="ruby-platform-procfile"></a>

To specify the command that starts your Ruby application, include a file called `Procfile` at the root of your source bundle\.

**Note**  
Elastic Beanstalk doesn't support this feature on Amazon Linux AMI Ruby platform branches \(preceding Amazon Linux 2\)\. Platform branches with names containing *with Puma* or *with Passenger*, regardless of their Ruby versions, precede Amazon Linux 2 and don't support the `Procfile` feature\.

For details about writing and using a `Procfile`, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

When you don't provide a `Procfile`, Elastic Beanstalk generates the following default file, which assumes you're using the pre\-installed Puma application server\.

```
web: puma -C /opt/elasticbeanstalk/config/private/pumaconf.rb
```

If you want to use your own provided Puma server, you can install it using a [Gemfile](ruby-platform-gemfile.md)\. The following example `Procfile` shows how to start it\.

**Example Procfile**  

```
web: bundle exec puma -C /opt/elasticbeanstalk/config/private/pumaconf.rb
```

If you want to use the Passenger application server, use the following example files to configure your Ruby environment to install and use Passenger\.

1. Use this example file to install Passenger\.  
**Example Gemfile**  

   ```
   source 'https://rubygems.org'
   gem 'passenger'
   ```

1. Use this example file to instruct Elastic Beanstalk to start Passenger\.  
**Example Procfile**  

   ```
   web: bundle exec passenger start /var/app/current --socket /var/run/puma/my_app.sock
   ```

**Note**  
You don't have to change anything in the configuration of the nginx proxy server to use Passenger\. To use other application servers, you might need to customize the nginx configuration to properly forward requests to your application\.