# Deploying On Premises Active Directory using Azure

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying On Premises Active Directory using Azure</h1>
In this lab we will be creating the infrastructure required to host an on premises deployment of Active Directory using Azure  

We will install nessary Virtual networks and Virtual machines as well as installing Active directory. 

Then we will create a client virtual machine that will act as an end user.  



<h2>Video Demonstration</h2>



<h2>Environments and Technologies Used</h2>



<h2>Operating Systems Used </h2>



<h2>High-Level Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/9e656ee5-e745-4ed0-9934-9088ce27b3cc)

1. Create an Azure Virtual Network (VNet)
2. Create a Windows Server VM
3. Install Active Directory
4. Promote Windows Server VM to a domain controller
6. Create Client VM
7. Configure DNS settings
8. Verify DNS settings on Client-1  


<h2>Deployment and Configuration Steps</h2>

### 1. Create an Azure Virtual Network (VNet)
- Navigate to Virtual Networks on Azure and click "create" as pictured below

  
![image](https://github.com/user-attachments/assets/32ef39ea-8bb9-493d-9e6d-512f20bc51af)

 
- Click "Create new" and create a new resource group called "AD-Hybrid-lab" <br>

- Name the Vnet "AD-Hybrid-VNET"

- Click "Review + create" and then click create again on the following screen to create the VNet <br>

> [!NOTE]
> Take note *of your region in this case I'm keeping it East US we will reference this in the next steps*

![image](https://github.com/user-attachments/assets/71ed9a7e-fa7e-4c61-9dc4-38bdade12583)




### 2. Deploy a Windows Server VM and place it in the Vnet
- Navigate to the Virtual machines page and click on create
  
![Pasted image 20250228195539](https://github.com/user-attachments/assets/8d6986e1-686c-4a5f-909c-57191158e23c)

Configure the following in the highlighted areas in the picture below:
- resource group:
   - Select the "AD-Hybrid-lab" group
- Virtual machine name:
    - Name it "Domain-Controller-1"
 - Region
    - ***Select same region as the vnet from the previous step in my case it is East US***
- Image:
   - select "Windows Server 2022 Datacenter: Azure Edition - x64 Gen2"
- Size: 
   - select "Standard_D4s_v3 - 4 vcpus, 16 GIB memory"
- Username and password
   - For lab purposes I chose Username: LabADMIN Password: LabPassword123

> [!IMPORTANT]
> Make sure the region matches the Vnet's region

![image](https://github.com/user-attachments/assets/eb11886e-5833-4df9-86cd-c0c0dbf8724f)


> [!IMPORTANT]
> Navigate to the "Networking" tab and Verify that AD-Hybrid-VNET is selected for virtual network

After you verify then "click "Review + create" and proceed to create it 

![image](https://github.com/user-attachments/assets/f39c9c10-9492-4f8b-8b9c-6dee3e2e3825)





### 3. Install Active Directory 
First we must RDP into Domain-Controller-1 to do that we need to find its public IP address

We can find the IP address by navigating to: 
 - Virtual Machines --> Domain-Controller-1 --> Networking --> Network settings
   
![image](https://github.com/user-attachments/assets/36c65ae5-cd40-42f9-ae4a-7f5e4c3c7b91)

- Next we will RDP in using Remote Desktop Connection using the public IP 
   - The login credential will be Username: LabADMIN Password: LabPassword123 from earlier

![image](https://github.com/user-attachments/assets/e2ed221a-4c8e-44bc-b203-4bfc1720ae35)

- When you connect you should be met with this screen
   -To begin installing Active Directory click on "add roles and files"
  
![418238521-7925871f-0282-4e5b-a517-ed33bbcd5fc4](https://github.com/user-attachments/assets/00bd1956-3a31-4375-ae36-3b3b285224c9)

- Next we will be met with the activation wizard we can click next on all prompt until we get to the "Server roles" secction

![image](https://github.com/user-attachments/assets/1dbe9ded-acb3-4b01-824a-4d1160ee089f)


 - On the "Server roles" section:
    -  check the highlighted "Active Directory Domain Services" box
    -  accept any addtional prompts and hit next until you get to "Confirmation"

![image](https://github.com/user-attachments/assets/e87b07f4-50f5-466b-9f5f-048fe84dc7e8)

 - Check the "Restart the destination server automatically if required" and click install
   
![image](https://github.com/user-attachments/assets/d28d5c80-7f28-455a-afe5-39b30ec4b33a)

- Now we must promote this VM to domain controller to do that
   - Select the flag icon on the top right of the screen and click on "Promote this server to a domain controller"

![image](https://github.com/user-attachments/assets/b0fe3318-9eee-4a6e-843f-86179d20f2f5)

- Enter "exampledomain.com" into the root domain name

![image](https://github.com/user-attachments/assets/9659ddb9-675e-4da1-98bf-c6e1529ff0bd)


- Next choose a password for the Dirrectory Services Restore Mode
   - I used: LabPassword123 to keep lab passwords consistent

![image](https://github.com/user-attachments/assets/e2aba41f-d5a8-4205-92d8-6dc92a67031e)

- uncheck "Create DNS delegation" when met with this prompt

![image](https://github.com/user-attachments/assets/5475c2e7-8c06-4af0-a8de-ee18385c9231)

- Click next until you get to Prerequisites Check and click install
   - After installtion the VM will restart and you will loose the RDP connection momentarily
      
![image](https://github.com/user-attachments/assets/c9b03c31-f37f-48c9-8f51-f09bb620aff4)

### Create Client VM
While we are waiting on the Domain-Controller-1 VM to restart this would be a good time to set up the client VM 

![image](https://github.com/user-attachments/assets/da25b6b7-1b01-4144-bc9a-8060aa9b5223)

![image](https://github.com/user-attachments/assets/bf057c79-9a3a-4804-a6f1-e1e9102313f8)

Set the VM to have a static IP to do that we must do the following:

Go to Virtual Machines --> Domain-Controller-1 --> Networking --> Network settings --> NIC --> IPconfig --> Private IP address settings --> select static and save 

Refer to the gif below: 

![ezgif-872af4bccc6f3e](https://github.com/user-attachments/assets/8962c61e-0581-4d65-887c-30373dcba181)

setting Client-1 DNS settings:

![cropped-client-1-dns-settings](https://github.com/user-attachments/assets/d0d6d329-9cf9-4cd5-96bd-a7f4700afc86)

Restart VM for DNS settings to apply

![image](https://github.com/user-attachments/assets/1a4d4a8c-07cb-4020-87c3-e1a3754cb1b9)

Disable firewall on Domain-Controller-1

![image](https://github.com/user-attachments/assets/797d96bb-6f3c-4394-a15a-c6c13d70d976)

![image](https://github.com/user-attachments/assets/a56da034-d9bd-4e29-b3aa-54a9d1d110c9)

Make sure you tunr off Domain Profile, Private profile, and Public profile

![image](https://github.com/user-attachments/assets/1a201109-b571-4b7b-bb4e-0fbd9b0a4402)





#### Client VM DNS Configuration

Before we create the VM it is important to understand what we will be doing to simplify this I created the diagram below

**On the left/orange section:** 
- This is what Azure deaults to when we create the client VM and when we created the Domain-Controller-1 VM 

**On the right/blue section:**
- What we will be configuring our Domain-Controller-1 VM and Client-1 VM

Do do this configuration we will need to do the following steps:
1. Configure a static IP on  Domain-Controller-1
2. Then we must tell Client-1 to use Domain-Controller-1 as its DNS server as well as joining it to the Active Directory Domain

![image](https://github.com/user-attachments/assets/dc6edbc0-9a9f-4a89-bcd1-3c5e3be7d484)


Connect to Client-1 
![image](https://github.com/user-attachments/assets/fac010a6-f9c2-40b7-af21-3e2544dd1d22)

login using RDP 
![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)

![image](https://github.com/user-attachments/assets/7917b1a7-e7cb-4396-801c-b41e39b9c58d)












