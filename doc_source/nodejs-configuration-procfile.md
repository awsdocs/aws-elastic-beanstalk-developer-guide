# Configuring the application process with a Procfile<a name="nodejs-configuration-procfile"></a>

You can include a file called `Procfile` at the root of your source bundle to specify the command that starts your application\.

**Example Procfile**  

```
web: node index.js
```

For information about `Procfile` usage, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

**Note**  
This feature replaces the legacy `NodeCommand` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.