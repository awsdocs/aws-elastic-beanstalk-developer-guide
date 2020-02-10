# Building executable on\-server with a Buildfile<a name="go-buildfile"></a>

To specify a custom build and configuration command for your Go application, include a file called `Buildfile` at the root of your source bundle\. The file name is case sensitive\. Use the following format for the `Buildfile`: 

```
<process_name>: <command>
```

The command in your `Buildfile` must match the following regular expression: `^[A-Za-z0-9_]+:\s*.+$`\.

Elastic Beanstalk doesn't monitor the application that is run with a `Buildfile`\. Use a `Buildfile` for commands that run for short periods and terminate after completing their tasks\. For long\-running application processes that should not exit, use the [Procfile](go-procfile.md) instead\.

In the following example of a `Buildfile`, `build.sh` is a shell script that is located at the root of the source bundle:

```
make: ./build.sh
```

All paths in the `Buildfile` are relative to the root of the source bundle\. If you know in advance where the files reside on the instance, you can include absolute paths in the `Buildfile`\.