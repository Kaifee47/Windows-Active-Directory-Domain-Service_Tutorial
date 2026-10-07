## Step-By-Step Guide

To get started, all you need is the following:
* Any old PC or laptop with at least:
  -  CPU with 2 to 4 cores
  -  Memory of 4 GB to 8 GB+ 
  -  Storage of 64 GB to 100 GB+
  -  Has Wifi or ethernet network adapter
Don't worry about complicated networks with 2 - 3 machines. We will use a Virtualization Software to create Virtual machines (VM) on the same old PC or laptop.

The following image shows how we are going to architect the network for this exercise.  

![Image describing how the final network architecture looks like](https://github.com/Kaifee47/Windows-Active-Directory-Domain-Service_Tutorial/blob/ec9fb900bed09bcad0fdc2119b6e982edebcc4c6/sources/img/DC%20setup.png)

We have the Domain Controller, or the server in layman's terms, sitting in the middle between the Client machine and the Router onwards to the Internet.
P.S. As you read on, some terms might feel unfamiliar and out of place, but I promise by the end of this exercise you will get the full picture.


### 1. Preparation
  
  1. We install an operating system, if there isn't one already installed. This is completely your choice. You can go for something familiar like Windows, or something lightweight and fast like a Linux Distribution. This is a good time to get familiar with and experiment with Linux if you aren't familiar already, since you are already trying to learn something new.
  
  2. Next, we will install a virtualization software. Notable options are Oracle VirtualBox or VMware. These software create a hosted hypervisor, and isolate the host OS from the guest OS. Just fancy way of saying you can't break your machine while messing with the Virtual Machine. My choice is VirtualBox for this exercise.
  
  3. We will also take this time to download another couple ISOs. First is the Windows Server's Operating System (OS), more specifically a server OS like Windows Server 2019. This is free to download and can be found on the Microsoft website with a quick Google search. We will also download the ISO for Windows 10 or 11. Since Windows ISOs contain the installation files for both Home and Pro versions, this makes our lives easier. We need the Pro version to connect to the domain.

  4. Once your choice of virtualization software is installed, we can begin creating two virtual machines. One will be the Domain controller. This is where we will set up the AD/DS and all the other fun stuff. The other will be a Windows 10 or 11 Pro virtual machine for simulating a client machine connecting to the domain.
		a. For the Domain Controller (DC), we install the server OS ISO. Remember to install **with** the Graphical interface.
		b. Since this is presumably the first time we are learning about Active Directory and Domain Services, we want the GUI. Once we are more advanced and/or more comfortable with the Command Line, we can try without the GUI.
		
  5. In the VirtualBox configuration menu while setting things up, or settings for each VM if you want to change things afterwards, we can choose resources each VM gets.
    
     a. We can choose 1 core for each of the virtual machine, with 1 to 2 GB at least for memory.

     b. We can also allocate memory dynamically since it will only use what it needs and we can set the Max value, in a sense, to be right about 30 to 45 GB for the DC and 15 to 30 GB for the Client.

     c. We can also allocate Network Interface Cards (NICs) to these machines. We want 2 NICs for the DC, one set to NAT and the other set to Internal Network(preferred) or Bridged Adapter. The client gets 1 NIC set to Internal or Bridged, whichever you chose for the DC's second NIC.  
  
  
  8. Now we are ready to move on to the next phase.


### 2. Setting up the DC for ADDS

By this point in time you should already have the 2 virtual machines with their respective OS installed.
Now we can start adding features, or *roles* as the Windows Server 2019 likes to call them.
In this phase we will set up the Active Directory and Domain Services as a role.

  1. To get started, we will configure the NICs we provided to the DC.
     We check the NICs settings by Right-clicking on the wifi or ethernet icon in the bottom right and going to {*Network & Internet settings > advanced network settings*}
     Look for the IPv4 address.
     
     a. The NIC connected to the Internet and DHCP provided IP from the router will look sort of familiar. Like the IP of your home network. For example, 192.168.1.224
     This is a common IP in the DHCP Home networks range. The other NIC will have a randomly assigned IP that the DC itself provided.

     b. We will rename the NICs to reflect the functions like "Internet" and "X--Internal--X". This will be helpful later.
     
     
  2. Go to the Server Manager, and add a role. Here we can choose ADDS as a role.


  3. From the notifications in the Server Manager, promote server to a domain controller and configure it

     
     a. Click Add to/Make Forest per your need. Since this is a exercise, we will make a new Forest and we will name it something simple, like "mydomain.com"
     
     b. Set a DSRM password (D.S. Restore Mode Password)
     This isn't usually used anytime soon, but it helps to understand that you might need to restore in the real world and this controls who gets that access.
     
     
 
  4. Use {Start>Windows Administration Tools>Active Directory Users and Computers} to make admin accounts
     
     a. Open new Organizational Unit and add User

     b. Change {User Properties>Member of} and Add DomainAdmins

P.S. We can use similar method to also add regular users. just create and add them to Organizational unit Users, instead of admins or DomainAdmins


We have the ADDS set up and we have our own admin account instead of the generic one we set up during OS installation. We can now move on to the next phase!


### 3. Setting up the RAS/NAT to allow Remote Connection

By this point in time you should already have the DC set up with AD/DS, its own forest, and your own Admin account.
Now we can start adding features like Remote Access to give Remote Connection and routing capabilities.
In this phase we will set up the Remote Access as a role.


  1. Go to the Server Manager, and add a role. Here we can choose Remote Access as a role.
  Remote Server Admin, Remote Access Tools and Web Server(IIS) + Windows Internal Database


  2. Select Routing in Role Services during Installation:
  This will also install RAS


  3. Use {Tools>Routing and Remote Access} in Windows Server Manager to set up NAT
     
     a. Configure and Enable Routing  on the DC server
     
     b. Use NAT on the wizard and choose NIC connected to Internet and **not** the Internal Network.

     
We have the RAS set up and we have what we need to move on to the next phase!


### 4. Setting up the DHCP to NAT clients to get IP and connect to the internet

You should already have the DC set up with RAS as a role
Now we can start adding features like DHCP to give Remote Connections the ability to get IP addresses and have access to the Internet.
In this phase we will set up the DHCP as a role.


  1. Go to the Server Manager, and add a role. Here we can choose DHCP Server as a role.

  
  2. In {DHCP>Tools} go to IPv4.
     
     a. Configure and add new scope. This should line up with the range shown in the Architecture Image in the beginning.
          
     b. Add router in server options for IPv4 if not already configured


     
Great! We now have the DHCP set up. Although I haven't provided much of an explanation for the IP configuration and the scopes, etc.; I believe those are huge topics that require a tutorial or lesson of their own.

I believe NetworkChuck has a great series on IP and subnetting on Youtube called "You suck at Subnetting" and that should help teach you about IPs and also entertain you a fair bit.


Now that we have what we need to move on to the next phase!


### 5. Setting up the client machine to be part of the domain and simulate a user

You should already have the DC set up with all the features we need and have gone over.

Now we can start adding clients to the network we worked so hard to set up.
In this phase we will set up and simulate a client machine.


  1. Minimize the Virtualbox Window for the DC and Go to the Client VM we created with Windows 10 or 11 Pro.
  
  2. Log in with any user account and in {System Properties>advanced System Settings>Computer Name}, change name AND domain of machine. 
     
     a. It **has** to be from the advanced settings or else it won't have the *option* to change or add to a domain.

     b. Once in the settings, change the name to something like "Client1" and change the domain to whatever you chose to name the domain.

     If you followed me exactly, from the beginning, the name should be "mydomain.com". It will likely ask for a restart. We are done after the restart.

     
Great! We now have the machine set up on the network and domain. 

We can use any User login that is already an user or admin or anything in the domains Active Directory.



#### Big Thanks to NetworkChuck and Josh Madakor for their amazing videos on this topic! 
#### I learned a lot from them that helped me visualize the information I was getting from books. 
#### Hope this helps to understand some of the steps and as a reference. Feel free to reach out if you have any questions and I would be more than happy to help!


