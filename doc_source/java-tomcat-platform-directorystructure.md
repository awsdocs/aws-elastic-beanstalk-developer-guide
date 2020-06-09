# Structuring your project folder<a name="java-tomcat-platform-directorystructure"></a>

To work when deployed to a Tomcat server, compiled Java Platform Enterprise Edition \(*Java EE*\) web application archives \(WAR files\) must be structured according to certain [guidelines](https://docs.oracle.com/javaee/7/tutorial/packaging003.htm)\. Your project directory doesn't have to meet the same standards, but it's a good idea to structure it in the same way to simplify compiling and packaging\. Structuring your project folder like the WAR file contents also helps you understand how files are related and how they behave on a web server\.

In the following recommended hierarchy, the source code for the web application is placed in a `src` directory, to isolate it from the build script and the WAR file it generates\.

```
~/workspace/my-app/
|-- build.sh            - Build script that compiles classes and creates a WAR
|-- README.MD           - Readme file with information about your project, notes
|-- ROOT.war            - Source bundle artifact created by build.sh
`-- src                 - Source code folder
    |-- WEB-INF         - Folder for private supporting files
    |   |-- classes     - Compiled classes
    |   |-- lib         - JAR libraries
    |   |-- tags        - Tag files
    |   |-- tlds        - Tag Library Descriptor files
    |   `-- web.xml     - Deployment Descriptor
    |-- com             - Uncompiled classes
    |-- css             - Style sheets
    |-- images          - Image files
    |-- js              - JavaScript files
    `-- default.jsp     - JSP (JavaServer Pages) webpage
```

The `src` folder contents match what you will package and deploy to the server, with the exception of the `com` folder\. The `com` folder contains your uncompiled classes \(`.java` files\)\. These need to be compiled and placed in the `WEB-INF/classes` directory to be accessible from your application code\.

The `WEB-INF` directory contains code and configurations that are not served publicly on the web server\. The other folders at the root of the source directory \(`css`, `images`, and `js`\) are publicly available at the corresponding path on the web server\.

The following example is identical to the preceding project directory, except that it contains more files and subdirectories\. This example project includes simple tags, model and support classes, and a Java Server Pages \(JSP\) file for a `record` resource\. It also includes a style sheet and JavaScript for [Bootstrap](http://getbootstrap.com/), a default JSP file, and an error page for 404 errors\.

`WEB-INF/lib` includes a Java Archive \(JAR\) file containing the Java Database Connectivity \(JDBC\) driver for PostgreSQL\. `WEB-INF/classes` is empty because class files have not been compiled yet\.

```
~/workspace/my-app/
|-- build.sh
|-- README.MD
|-- ROOT.war
`-- src
    |-- WEB-INF
    |   |-- classes
    |   |-- lib
    |   |   `-- postgresql-9.4-1201.jdbc4.jar
    |   |-- tags
    |   |   `-- header.tag
    |   |-- tlds
    |   |   `-- records.tld
    |   `-- web.xml
    |-- com
    |   `-- myapp
    |       |-- model
    |       |   `-- Record.java
    |       `-- web
    |           `-- ListRecords.java
    |-- css
    |   |-- bootstrap.min.css
    |   `-- myapp.css
    |-- images
    |   `-- myapp.png
    |-- js
    |   `-- bootstrap.min.js
    |-- 404.jsp
    |-- default.jsp
    `-- records.jsp
```

## Building a WAR file with a shell script<a name="java-tomcat-platform-directorystructure-building"></a>

`build.sh` is a very simple shell script that compiles Java classes, constructs a WAR file, and copies it to the Tomcat `webapps` directory for local testing\.

```
cd src
javac -d WEB-INF/classes com/myapp/model/Record.java
javac -classpath WEB-INF/lib/*:WEB-INF/classes -d WEB-INF/classes com/myapp/model/Record.java
javac -classpath WEB-INF/lib/*:WEB-INF/classes -d WEB-INF/classes com/myapp/web/ListRecords.java

jar -cvf ROOT.war *.jsp images css js WEB-INF
cp ROOT.war /Library/Tomcat/webapps
mv ROOT.war ../
```

Inside the WAR file, you find the same structure that exists in the `src` directory in the preceding example, excluding the `src/com` folder\. The `jar` command automatically creates the `META-INF/MANIFEST.MF` file\.

```
~/workspace/my-app/ROOT.war
|-- META-INF
|   `-- MANIFEST.MF
|-- WEB-INF
|   |-- classes
|   |   `-- com
|   |       `-- myapp
|   |           |-- model
|   |           |   `-- Records.class
|   |           `-- web
|   |               `-- ListRecords.class
|   |-- lib
|   |   `-- postgresql-9.4-1201.jdbc4.jar
|   |-- tags
|   |   `-- header.tag
|   |-- tlds
|   |   `-- records.tld
|   `-- web.xml
|-- css
|   |-- bootstrap.min.css
|   `-- myapp.css
|-- images
|   `-- myapp.png
|-- js
|   `-- bootstrap.min.js
|-- 404.jsp
|-- default.jsp
`-- records.jsp
```

## Using `.gitignore`<a name="java-tomcat-platform-gitignore"></a>

To avoid committing compiled class files and WAR files to your Git repository, or seeing messages about them appear when you run Git commands, add the relevant file types to a file named `.gitignore` in your project folder\.

**\~/workspace/myapp/\.gitignore**

```
*.zip
*.class
```