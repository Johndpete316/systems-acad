# Lab Goals

- Create a reproducible training environment for use during the GLR Cybersecurity Academy. 
- Gain a basic understanding of what terraform is, and how it works. 
- Learn essential troubleshooting skills that will transfer well into any computer science related field

**see the network diagram at the bottom of the page for a clear view of what we are building**



# Azure Range Setup Guide

The following instructions will explain how to build, and maintain the Azure Range. PurpleCloud uses a number of technologies to automate the building of the lab, and anything we don't cover, feel free to lookup for yourself. The main ones we will be focusing on are Azure and Terraform.





# Seting up the environment. 

## Create a working directory. 

On windows, open up the command prompt. We will be using (windows terminal or WSL). 

The current directory should be your users home directory `C:\Users\GLRadmin\`. You can see this by typing `pwd` in the terminal window. 

Now create a new folder, this will be your working directory for the lab. I will be using `AzureLab`. 

`mkdir AzureLab`

Now move into that folder with `cd`

`cd AzureLab`

You can type `pwd` again to see that your current 'working directory' has changed.

## Clone the PurpleCloud Repository

The PurpleCloud repository by @iknowjason is a set of tools used to generate terraform files that are then used to generate the labs. Think of it as a template that we will use to generate scripts to be applied to Azure later on in this lab. 

`git clone https://github.com/iknowjason/PurpleCloud`

`cd PurpleCloud`


## Download and install terraform

First, download the terraform executable for windows 64-bit.  

https://www.terraform.io/downloads

Now, using either the windows file explorer or the command line, move the terraform executable into the recently downloaded PurpleCloud repository.

For the rest of the lab if you need to run a terraform command you can use `.\terraform.exe <command>`

## Next we need to log in to azure. You each will have received an azure username and login. 

Your username will be in the following format 

`First.Lastname@xyz.onmicrosoft.com`

And you will be given a default random password which you will have to change at signin. 
Please skip any prompts regarding 2 Factor Authentication.

To log into the Azure CLI tool, we can run

`az login`
```
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code \<One Time Passcode\> to authenticate.
```

Once the webpage confirms the signon is complete, you can close it out and test your account. 

`az account show`
```
{
  "environmentName": "AzureCloud",
  "homeTenantId": "...",
  "id": "...",
  "isDefault": true,
  "managedByTenants": [],
  "name": "...",
  "state": "Enabled",
  "tenantId": "...",
  "user": {
    "name": "testuser@xyz.onmicrosoft.com",
    "type": "user"
  }
}
```

## Required python libraries. 

Finally we will want to install faker, a python library required by the script used to generate the terraform files. 

`python3 -m pip install faker`



# Creating the lab

First create the environment using `ad.py`. You want to make sure for our setup during the week you have all of these switches enabled on the command. For the `resource_group` please replace First-Lastname with your name

For more information on what each switch does, you can see the PurpleCloud documentation. 
https://www.purplecloud.network/usage/#advanced-usage


## Create the terraform template files

```
python3 ad.py --domain_controller --ad_domain MrRobot.local --admin GLRadmin --password Cyber2022! --ad_users 50 --endpoints 3 --location eastus --domain_join --helk --resource_group First-Lastname
```

## Initialize Terraform

Now that the terraform files are generated we are going to run some terraform specific commands using the CLI we installed earlier. 

`$ .\terraform.exe init`

```
Initializing the backend...

Initializing provider plugins...
...


Terraform has been successfully initialized!
```

## Creating a .plan file

Now we can create a plan file using terraform plan. Everytime you make changes to the terraform files, you need to re-run this command (for instance if you need to re-run the ad.py file due to missing option.)


`$ .\terraform.exe plan -out out.plan`

this will generate a zip file called `out.plan`

## Apply to Azure

Finally, we can apply these templates to azure using `teraform apply`.

`$ .\terraform.exe apply out.plan`

With our current setup this could take anywhere from 20 - 30 minutes. Do not close the terminal window or shut down the machine you ran the command from. This is where you are most likely to run into errors. If anything comes up, take your time and read it slowly. Google any keywords you see. If you are having a hard time deciphering it, ask an instructor for help. 



# testing the lab environment

Finally we need to test the lab environment. 

## Locating a machines public IP address


1. Expand the toolbar on the lefthand side. 
2. Click on resource groups.
3. Select the resource group you created. 
4. Find the `velocihelk` machine. 
5. Locate the machines `Public IP address`. (should begin with a 20)

## Try accessing Kabana UI. 

`In MS Edge go to https://\<velocihelk-public-ip>`

## Try accessing velociraptor

`In MS Edge go to https://\<velocihelk-public-ip>:8889` 

## Accessing the domain Controller

1. Expand the toolbar on the lefthand side. 
2. Click on resource groups.
3. Select the resource group you created. 
4. Find the `dc1` machine. 
5. In the top left click `connect`. 
6. Select `RDP` from the dropdown menu. 
7. Open the downloaded file. 

**Note: Anytime you destroy and create the environment all device public IPs change meaning you will have to remember and re-use this process throughout the week. You can always come back here for a reference if needed.**


# Destroy the Lab

If any bugs come up, or the lab somehow breaks during the week (*we will make sure that happens* 😉) You can easily destroy the lab. 

1. Make sure you are in the working directory where the lab was created from (Ex. `C:\Users\GLRadmin\AzureLab`)
2. Run `.\terraform.exe destroy`
3. Enter `yes` when prompted. 

# Network Diagram

![Network Diagram](https://www.purplecloud.network/images/pce.png)

# Credits

@iknowjason and the PurpleCloud project for powering this lab.
Huge thanks for the help and support thus far. 




