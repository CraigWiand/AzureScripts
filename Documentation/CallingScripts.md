There are several ways the scripts in this repo can be run. Below is a description of some of the most common patterns used to invoke Azure support scripts.

##Running script on a local computer
This is the most straightforward pattern, so we will start here.  Scripts designed to run on your local computer will be copied/downloaded/cloned from GitHub and run on the computer you are using.  In this pattern, the local computer will need to have all of the necessary runtimes and dependencies for the script to execute, this includes operating system and scripting environments. It will also need any appropriate network connectivity.

##Running script on a virtual machine
In this pattern, the script is run on a virtual machine(VM) running in Azure. In this pattern, the VM in Azure will need to have all of the necessary runtimes and dependencies for the script to execute, this includes operating system and scripting environments. It will also need any appropriate network connectivity.

##Invoking a script through CustomScriptExtension
The custom script extension allows you to run scripts on a remote VM, without logging into it. The scripts can be run after provisioning the VM or any time during the lifecycle of the VM without requiring to open any additional ports on the VM. The custom script extension is available for both [Windows virtual machines](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-extensions-customscript/) as well as [Linux virtual machines](https://azure.microsoft.com/en-us/blog/automate-linux-vm-customization-tasks-using-customscript-extension/).  The custom script extension will automatically download and execute these scripts for you, from the location you’ve specified, with parameters you’ve entered.  This makes it an excellent choice for running scripts from public script repositories (such as this one in GitHub). 

There are a couple ways to use the custom script extension, including:

###Using CustomScriptExtension via Azure Portal
To use the custom script extension from the [Azure portal](http://portal.azure.com) browse to the VM you would like to run the script on.  Then in Settings choose Extensions, and add the extension by clicking on the '+' sign and choosing Create.  Then specifying the script file location and command to run.
![portal_image](https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-extensions-customscript/20160415071221/addcse.png)

###Using a script to use CustomScriptExtension
In some scenarios, the parameters that a script requires are difficult to acquire or complex to pass.  An example of this would be a scenario where a scripted needed to use a generated[shared access signature URI ](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-2/) to a storage container.  Given the number of manual steps needed, it is desirable to automate the creation of the SAS URI and then have that script pass it to another script which used the URI in this work.

Scripts in this repository which target similar complex scenarios may have two scripts.  One script runs on the local computer and gathers the necessary parameters, the second script uses those parameters to do the work.


-Note: The design of Azure CustomScriptExtension framework does not allow you to run the same script repeatedly with the same parameters.  For this reason, you will see many of the scripts in this repository use a timestamp as a parameter.  This timestamp may be helpful in logging, but also provides uniqueness across script invocations.-
