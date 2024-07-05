# Forenote
Images seem small in the page, but can be clicked to expand. 

# Project Overview
This project is based on Grant Collins's ["Basic Home Lab Running Active Directory"](https://youtu.be/MHsI8hJmggI?si=m6b_8RZaJnNUKuKr). This project will utilize VirtualBox to create a domain controller running Windows Server 2019 which houses our Active Directory. This will be configured with network adapters. It will have NAT and routing configured, as well as possess DHCP for automatic IP addressing. We will also run a PowerShell script to create a thousand users. Another virtual machine will be created that runs Windows 10 Enterprise which will connect to our domain controller. 

# Setup
To virtualize a Windows 2019 Server and Windows 10 Enterprise system, we will need their respective .iso files. To begin, we will obtain these. 
![win10ISO](https://github.com/mkathia/malware-analysis/assets/113075504/27755319-71da-48d1-ad18-027e8360d9d2)
![image](https://github.com/mkathia/ad-lab/assets/113075504/75168945-8dd7-48d7-83cd-9a5350652d44)

# Server Configuration
After initially configuring the server, we also want to make modifications to our network settings. Namely, we want to ensure that have our standard connection, as seen below:
![image](https://github.com/mkathia/ad-lab/assets/113075504/116c9630-12a2-479e-9753-fcc4286a8389)

But we also want to enable another network adapter for our internal network.
![image](https://github.com/mkathia/ad-lab/assets/113075504/349be254-a6a8-4a81-ace9-568e9993aded)

Then, we begin our configuration setups.
![image](https://github.com/mkathia/ad-lab/assets/113075504/d556dc94-571f-43a8-ba2f-ef76ab6385eb)

We pick Desktop Experience for the GUI.
![image](https://github.com/mkathia/ad-lab/assets/113075504/0e275e78-c461-4b62-bbad-0d337d5bf15d)

We choose custom install,
![image](https://github.com/mkathia/ad-lab/assets/113075504/a68da0b4-8bdf-4af4-a1c1-ef491cc064b5)

And then create our profile. After doing so, we are put into the server manager.
![image](https://github.com/mkathia/ad-lab/assets/113075504/aaf07b3e-bb05-43b3-a497-6ce72357b665)

Our next step is now to configure the internal network. We go into network settings.
![image](https://github.com/mkathia/ad-lab/assets/113075504/c73d22ba-d845-4830-8646-afde368c1517)

We need to figure out what each of these are and name them appropriately for later use. We click on the first one and expand details:
![image](https://github.com/mkathia/ad-lab/assets/113075504/9080093c-3b39-459e-a2ab-4abe1c3977ca)

We can see this is our standard home IP address for internet. Thus, we name this one internet. 

When looking at the details for the second one, we can see that it's the internal network.
![image](https://github.com/mkathia/ad-lab/assets/113075504/be7d7ed6-7681-4caa-a050-a3874b75678c)

Thus, we name it such. We also assign an IP address and a DNS server address.
![image](https://github.com/mkathia/ad-lab/assets/113075504/26899433-6829-4ab3-85c9-75f021669d0e)

After doing such, we rename our PC to DC (Domain Controller).
![image](https://github.com/mkathia/ad-lab/assets/113075504/1dd5331e-e05a-4495-8e39-905446b44f15)

After restarting, we can proceed to establishing Active Directory. In Server Manager, we click "Add roles and features". I'm going to only post screenshots of changes made, if screenshot is not present, assume that default settings were kept.
Here we choose "Active Directory Domain Services"
![image](https://github.com/mkathia/ad-lab/assets/113075504/5cfc9e4c-4099-4b46-9bb8-5bb964631895)

After finishing the install, we notice a flag notification. Upon clicking it, we can see that we are being prompted to promote the server to a domain controller, which we can go ahead and do. I'm again going to post a series of screenshots.
![image](https://github.com/mkathia/ad-lab/assets/113075504/04c29f06-0cee-4761-a741-e3d831bd2ab8)
![image](https://github.com/mkathia/ad-lab/assets/113075504/6b1d280b-2c05-46db-b6db-3e49b12b9fb6)

The computer will then attempt to install prerequisites. After doing so, the computer automatically restarts. Once it's done, note that there is now a "MYDOMAIN\" before Administrator. This denotes that instillation is successful.
![image](https://github.com/mkathia/ad-lab/assets/113075504/b68517bc-3eaa-49dc-abcc-b84b95e92726)

After logging in, we're going to create our own administrative account instead of using the built in one. We'll do this by going through the start menu to "Windows Administrative Tools"/"Active Directory Users and Computers".
![image](https://github.com/mkathia/ad-lab/assets/113075504/cfcc545d-6da2-460e-82c4-d6a9d84851c8)

We create a new Organizational Unit (OU) in our domain named Admins. We create a user for ourselves inside this unit.
![image](https://github.com/mkathia/ad-lab/assets/113075504/d074948a-a7ab-48ba-a986-c1e134892ca9)

After creating our user, we add ourselves to the Domain Admins group by going into properties, going to "Member Of", and adding to aforementioned group.
![image](https://github.com/mkathia/ad-lab/assets/113075504/deb04e9f-304c-4772-be94-6f0c0686ea6a)

We then sign out, and log into "Other User" with our newly created account. As you can see, I am now in my own account as opposed to a default administrative account.
![image](https://github.com/mkathia/ad-lab/assets/113075504/418389d5-c630-4ac6-a573-8308beaf56e6)

Our next step is to set up NAT. We do this by going into Server Manager, and then clicking on "Add roles and features". Assume default options if screenshots are not provided.
Here, we click on "Remote Access".
![image](https://github.com/mkathia/ad-lab/assets/113075504/2636c3d7-4379-401d-ad93-d84abb0aa92b)

Here, we add "Routing"
![image](https://github.com/mkathia/ad-lab/assets/113075504/73eee341-0ae3-4552-bb58-0080ab29107e)

After completing the install, we can go to "Tools"/"Routing and Remote Access"
![image](https://github.com/mkathia/ad-lab/assets/113075504/b9932b61-a6b5-4f0b-b50d-aaa0013b08f5)

Then, we right click on "DC" and select the configure option.
![image](https://github.com/mkathia/ad-lab/assets/113075504/25c0cf6b-06fc-4d3c-aab5-694d650fc45f)

We want to install "NAT"
![image](https://github.com/mkathia/ad-lab/assets/113075504/665ffb1c-26d3-4f64-b10d-d90f20b50fa3)

We then want to use a public interface, and select our Internet interface. 
![image](https://github.com/mkathia/ad-lab/assets/113075504/116319a2-35a3-4f8d-991c-caa6ceaad2a6)

After this finishes, we can see all the new options that appear.
![image](https://github.com/mkathia/ad-lab/assets/113075504/015ff7ad-a710-4859-8691-fc214be16f08)

We now want to set up our DHCP. To do so, we go back to "Add roles and features". Note that the server name changed to DC.mydomain.com
![image](https://github.com/mkathia/ad-lab/assets/113075504/499cc71b-0662-40e9-b455-922ed26e4ddb)

Here we select DHCP Server, and Add features. We then proceed to install.
![image](https://github.com/mkathia/ad-lab/assets/113075504/dc9c8482-b2da-4333-8730-edde62ddf1cc)

After instillation is completed, we work towards setting up our scope. We go to the DHCP control panel, click IPv4, and select "New Scope".
![image](https://github.com/mkathia/ad-lab/assets/113075504/21e95e9a-222c-4ece-8539-0057a8d5bf19)

We name the scope.
![image](https://github.com/mkathia/ad-lab/assets/113075504/17d329ec-0371-4491-aa8a-c0b995f3e5f1)

Then, we set the start and end IP addresses and set a mask of 24.
![image](https://github.com/mkathia/ad-lab/assets/113075504/46f6e642-2407-4e67-a8d5-605a9131b213)

We continue past the next pages, Exclusions and Length Duration. We leave both as default.

![image](https://github.com/mkathia/ad-lab/assets/113075504/ca7fbd13-fec7-4489-9562-55bc800749e1)
![image](https://github.com/mkathia/ad-lab/assets/113075504/176d574a-766a-4a1c-ba7c-3458829c8122)

Then, we configure our DHCP options.
![image](https://github.com/mkathia/ad-lab/assets/113075504/1518b20d-c4b0-4488-ac67-305a59eff197)
![image](https://github.com/mkathia/ad-lab/assets/113075504/d23cbdaf-3ae9-46f3-ba07-6d532e0e320d)

We add the address of the domain controller as the default gateway.

The following are left as default.
![image](https://github.com/mkathia/ad-lab/assets/113075504/1a496f07-3aa8-4494-a8b5-88bd3aa36c01)
![image](https://github.com/mkathia/ad-lab/assets/113075504/bd765a18-7b0c-46d1-941d-721ee9775327)
![image](https://github.com/mkathia/ad-lab/assets/113075504/cac9dc23-5aad-451e-8096-375d8f06700a)

We then authorize our DHCP server.
![image](https://github.com/mkathia/ad-lab/assets/113075504/29af9298-f5f4-437c-92fa-1f3bd0436942)

As we can see, they turned green and we can see our newly created scope under IPv4.
![image](https://github.com/mkathia/ad-lab/assets/113075504/338f9eb0-5ba9-41e7-975f-53fc32588cdc)


Next, we're going to make a configuration that allows us to browse the internet from the domain controller. This isn't usually done, but we're doing it in the lab for convenience. We click "Configure this local server"
![image](https://github.com/mkathia/ad-lab/assets/113075504/f69f6f19-3bb2-4f21-9906-3900fec88614)

We're going to disable "IE Enhanced Security Configuration."
![image](https://github.com/mkathia/ad-lab/assets/113075504/813122cf-c59a-4d6e-9219-ad50ac15a91e)
![image](https://github.com/mkathia/ad-lab/assets/113075504/94ce1a8c-916c-4f9b-b69d-233c35d8dd1e)

Now, we're going to use a PowerShell script to create a large amount of users so we have something to work with. The source code is [here](https://github.com/joshmadakor1/AD_PS).

We get the script and extract it to our desktop.
![image](https://github.com/mkathia/ad-lab/assets/113075504/57837d03-4753-47db-a3c1-40b919ba07c9)



