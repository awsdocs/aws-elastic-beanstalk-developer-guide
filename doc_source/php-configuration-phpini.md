# Extending php\.ini<a name="php-configuration-phpini"></a>

Use a configuration file with a `files` block to add a `.ini` file to `/etc/php.d/` on the instances in your environment\. The main configuration file, `php.ini`, pulls in settings from files in this folder in alphabetical order\. Many extensions are enabled by default by files in this folder\.

**Example \.ebextensions/mongo\.config**  

```
files:
  "/etc/php.d/99mongo.ini" :
    mode: "000755"
    owner: root
    group: root
    content: |
      extension=mongo.so
```