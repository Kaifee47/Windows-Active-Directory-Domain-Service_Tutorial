## Step-By-Step Guide

To get started, all you need is the following:
* Any old PC or laptop with at least:
  -  2 to 4 cores
  -  4 GB to 8 GB+ 
  -  64 GB to 100 GB+
  -  1 Gbps network adapter with fixed/static IP address
Don't worry about complicated networks with 2 - 3 machines. We will use a Virtualization Software to create Virtual machines (VM) on the same old PC or laptop.

### 1. Preparation
  
  1. We install an operating system, if there isn't one already installed. This is completely your choice.
  You can go for something familiar like Windows, or something lightweight and fast like a Linux Distribution.
  This is a good time to experiment with Linux if you aren't familiar already, since you are already trying to learn something new.
  
  2. Next, we will install a virtualization software. Notable options are Oracle VirtualBox or VMware. These software create a hosted hypervisor,
  and isolate the host OS from the guest OS. Just fancy way of saying you can't break your machine while messing with the Virtual Machine.
  My choice is VirtualBox for this tutorial.
  
  3. Once your choice of virtualization software is installed, we can begin creating two virtual machines. One will be the Domain controller. This is where we will
  set up the AD/DS and all the other fun stuff. The other will be a regular windows virtual machine for simulating a client machine connecting to the domain.
		a. For the Domain Controller (DC), we install the Operating System (OS), more specifically a server OS like Windows Server 2019. This is free to download and can be found on the Microsoft website with a quick Google search.
		b. Remember to install with the Graphical interface. Since this is presumably the first time we are learning about servers and AD/DS, we want the GUI. Once we are more advanced and/or more comfortable with the Command Line, we can try without the GUI.
	
	4. Now we are ready to move on to the next step.

### 2. Setting up the DC for ADDS



Install server OS ISO
Configure Network Adapters + Set Static IP for Internal NIC
Install ADDS as role
Promote server to a domain controller and configure it
Add to/Make Forest (i.e. mydomain.com)
Set DSRM password (D.S. Restore Mode Password)
Use {Start>Windows Administration Tools>Active Directory Users and Computers} to make admin accounts
Open new Organizational Unit and add User
Change {User Properties>Member of} and Add DomainAdmins

Install Remote Access as a role: This will also install Remote Server Admin, Remote Access Tools and Web Server(IIS) + Windows Internal Database
Select Routing in Role Services during Installation: Automarically adds RAS
Use {Tools>Routing and Remote Access} in Windows Server Manager to set up NAT
Configure and Enable Routing  on the DC server
Use NAT on the wizard and choose NIC connected to Internet and not the Internal Network

Install DHCP Server as a role
In {DHCP>Tools} go to IPv4 and add new scope and then add router in server options for IPv4 if not already configured.

Connect client machine to hosted/internal network.
In {System Properties>advanced System Settings>Computer Name}, change name AND domain of machine.
Use any User login that is already user or admin or anything in the domain



