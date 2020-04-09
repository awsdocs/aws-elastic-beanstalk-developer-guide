# Specifying dependencies using a requirements file<a name="python-configuration-requirements"></a>

A typical Python application has dependencies on other third\-party Python packages\. With the Elastic Beanstalk Python platform, you have a few ways to specify Python packages that your application depends on\.

## Use `pip` and `requirements.txt`<a name="python-configuration-requirements.txt"></a>

The standard tool for installing Python packages is `pip`\. It has a feature that allows you to specify all the packages you need \(as well as their versions\) in a single requirements file\. For more information about the requirements file, go to [Requirements File Format](https://pip.pypa.io/en/latest/reference/pip_install.html#requirements-file-format)\.

Create a file named `requirements.txt` and place it in the top\-level directory of your source bundle\. The following is an example `requirements.txt` file for Django\.

```
Django==1.11.3
MySQL-python==1.2.5
```

In your development environment, you can use the `pip freeze` command to generate your requirements file\.

```
~/my-app$ pip freeze > requirements.txt
```

To ensure that your requirements file only contains packages that are actually used by your application, use a [virtual environment](python-development-environment.md#python-common-setup-venv) that only has those packages installed\. Outside of a virtual environment, the output of `pip freeze` will include all `pip` packages installed on your development machine, including those that came with your operating system\.

**Note**  
On Amazon Linux AMI Python platform versions, Elastic Beanstalk doesn't natively support Pipenv or Pipfiles\. If you use Pipenv to manage your application's dependencies, run the following command to generate a `requirements.txt` file\.  

```
~/my-app$ pipenv lock -r > requirements.txt
```
To learn more, see [Generating a requirements\.txt](https://pipenv.readthedocs.io/en/latest/advanced/#generating-a-requirements-txt) in the Pipenv documentation\.

## Use Pipenv and `Pipfile`<a name="python-configuration-requirements.pipenv"></a>

Pipenv is a modern Python packaging tool\. It combines package installation with the creation and management of a dependency file and a virtualenv for your application\. Pipenv maintains two files: `Pipfile` contains various types of dependencies and requirements, and `Pipfile.lock` is a version snapshot that enables deterministic builds\. For more information, see [Pipenv: Python Dev Workflow for Humans](https://pipenv.readthedocs.io/en/latest/)\.

Amazon Linux 2 Python platform versions support Pipenv\-based requirements files\. Create them on your development environment and include them with the source bundle that you deploy to Elastic Beanstalk\.

**Note**  
Amazon Linux AMI Python platform versions \(preceding Amazon Linux 2\) don't support Pipenv and `Pipfile`\.

The following example uses Pipenv to install Django and the Django REST framework\.

```
~/my-app$ pipenv install django
~/my-app$ pipenv install djangorestframework
```

These commands create the files `Pipfile` and `Pipfile.lock`\. Place `Pipfile` in the top\-level directory of your source bundle to get latest versions of dependency packages installed on your environment instances\. Alternatively, include `Pipfile.lock` to get a constant set of package versions reflecting your development environment at the time of the file's creation\.

If you include more than one of the requirements files described here, Elastic Beanstalk uses just one of them\. The following list shows the precedence, in descending order\.

1. `requirements.txt`

1. `Pipfile.lock`

1. `Pipfile`