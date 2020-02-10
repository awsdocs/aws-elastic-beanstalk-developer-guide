# Platform definition archive contents<a name="custom-platforms-pda"></a>

A platform definition archive is the platform equivalent of an [application source bundle](applications-sourcebundle.md)\. The platform definition archive is a ZIP file that contains a platform definition file, a Packer template, and the scripts and files used by the Packer template to create your platform\.

**Note**  
When you use the EB CLI to create a custom platform, the EB CLI creates a platform definition archive from the files and folders in your platform repository, so you don't need to create the archive manually\.

The platform definition file is a YAML\-formatted file that must be named `platform.yaml` and be in the root of your platform definition archive\. See [Creating a custom platform](custom-platforms.md#custom-platform-creating) for a list of required and optional keys supported in a platform definition file\.

You don't need to name the Packer template in a specific way, but the name of the file must match the provisioner template specified in the platform definition file\. See the official [Packer documentation](https://www.packer.io/docs/templates/introduction.html) for instructions on creating Packer templates\.

The other files in your platform definition archive are scripts and files used by the template to customize an instance before creating an AMI\.