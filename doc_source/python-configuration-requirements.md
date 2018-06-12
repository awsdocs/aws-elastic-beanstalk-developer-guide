# Requirements File<a name="python-configuration-requirements"></a>

Create a **requirements\.txt** file and place it in the top\-level directory of your source bundle\. A typical Python application will have dependencies on other third\-party Python packages\. In Python, pip is the standard way of installing packages\. Pip has a feature that allows you to specify all the packages you need \(as well as their versions\) in a single requirements file\. For more information about the requirements file, go to [Requirements File Format](https://pip.pypa.io/en/latest/reference/pip_install.html#requirements-file-format)\. The following is an example requirements\.txt file for Django\.

```
Django==1.11.3
MySQL-python==1.2.5
```

In your development environment, you can use the `pip freeze` command to generate your requirements file\.

```
~/my-app$ pip freeze > requirements.txt
```

To ensure that your requirements file only contains packages that are actually used by your application, use a [virtual environment](python-development-environment.md#python-common-setup-venv) that only has those packages installed\. Outside of a virtual environment, the output of `pip freeze` will include all pip packages installed on your development machine, including those that came with your operating system\.