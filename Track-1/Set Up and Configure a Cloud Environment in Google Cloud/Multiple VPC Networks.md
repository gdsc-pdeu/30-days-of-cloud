# Quest-3: Set Up and Configure a Cloud Environment in Google Cloud
## Lab-3: Multiple VPC Networks

<details> 
  <summary><b>Task 1: Create custom mode VPC networks with firewall rules</b></summary>
  <br/>
  <p>
    
1. Create the managementnet network.
    
    a. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > VPC network > VPC networks.
    
    b. Notice the default and mynetwork networks with their subnets.
    
    c. Click Create VPC Network.
    
    d. Set the Name to managementnet.
    
    e. For Subnet creation mode, click Custom.
    
    f. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | managementsubnet-us |
    | Region | us-central1 |
    | IP address range | 10.130.0.0/20 |
    
    g. Click Done.
    
    h. Click EQUIVALENT COMMAND LINE.
    
    k. Click Close. Click Create.
    
2. Create the privatenet network
    a. Run the following command to create the privatenet network:
    ```
    gcloud compute networks create privatenet --subnet-mode=custom
    ```
    b. Run the following command to create the privatesubnet-us subnet:
    ```
    gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24
    ```
    c. Run the following command to create the privatesubnet-eu subnet:
    ```
    gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west4 --range=172.20.0.0/20
    ```
    d. Run the following command to list the available VPC networks:
    ```
    gcloud compute networks list
    ```
    e. Run the following command to list the available VPC subnets (sorted by VPC network):
    ```
    gcloud compute networks subnets list --sort-by=NETWORK
    ```
    f. In the Cloud Console, navigate to Navigation menu > VPC network > VPC networks.
    g. You see that the same networks and subnets are listed in the Cloud Console.
3. Create the firewall rules for managementnet
    
    a. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > VPC network > Firewall.
    
    b. Click + Create Firewall Rule.
    
    d. Set the following values, leave all other values at their defaults:
    
    | Property|  Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | managementnet-allow-icmp-ssh-rdp |
    | Network | managementnet |
    | Targets	| All instances in the network |
    | Source filter | IP Ranges |
    | Source IP ranges | 0.0.0.0/0 |
    | Protocols and ports | Specified protocols and ports, and then check tcp, type: 22, 3389; and check Other protocols, type: icmp. |
    
    e. Click EQUIVALENT COMMAND LINE.

    f. Click Close.

    g. Click Create.
    
4. Create the firewall rules for privatenet
    a. In Cloud Shell, run the following command to create the privatenet-allow-icmp-ssh-rdp firewall rule:
    ```
    gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0
    ```
    b. Run the following command to list all the firewall rules (sorted by VPC network):
    ```
    gcloud compute firewall-rules list --sort-by=NETWORK
    ```
    c. In the Cloud Console, navigate to Navigation menu > VPC network > Firewall.
    d. You see that the same firewall rules are listed in the Cloud Console.
 
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create VM instances</b></summary>
  <br/>
  <p>
    
1. Create the managementnet-us-vm instance
    
    a. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.

    b. Click Create instance.

    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified)
    | Name | managementnet-us-vm |
    | :---: | :---: |
    | Region | us-central1 |
    | Zone | us-central1-f |
    | Series| N1 |
    | Machine type | 1 vCPU (f1-micro) |
    
    d. Click NETWORKING, DISKS, SECURITY, MANAGEMENT, SOLE-TENANCY.

    e. Click Networking.

    f. For Network interfaces, click the dropdown to edit.
    
    g. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Network |	managementnet |
    | Subnetwork | managementsubnet-us |
    
    h. Click Done. Click EQUIVALENT COMMAND LINE.

    i. Click Close.

    j. Click Create.


2. Create the privatenet-us-vm instance
    a. In Cloud Shell, run the following command to create the privatenet-us-vm instance:
    ```
    gcloud compute instances create privatenet-us-vm --zone=us-central1-f --machine-type=n1-standard-1 --subnet=privatesubnet-us
    ```
    b. Run the following command to list all the VM instances (sorted by zone):
    ```
    gcloud compute instances list --sort-by=ZONE
    ```
    c. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    d. You see that the VM instances are listed in the Cloud Console.
    e. Click on Column display options, then select Network. Click Ok.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Explore the connectivity between VM instances</b></summary>
  <br/>
  <p>
    
1. Ping the external IP addresses
    a. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.
    b. Note the external IP addresses for mynet-eu-vm, managementnet-us-vm, and privatenet-us-vm.
    c. For mynet-us-vm, click SSH to launch a terminal and connect.
    d. To test connectivity to mynet-eu-vm's external IP, run the following command, replacing mynet-eu-vm's external IP:
    ```
    ping -c 3 Enter mynet-eu-vm's external IP here
    ```
    e. To test connectivity to managementnet-us-vm's external IP, run the following command, replacing managementnet-us-vm's external IP:
    ```
    ping -c 3 Enter managementnet-us-vm's external IP here
    ```
    f. To test connectivity to privatenet-us-vm's external IP, run the following command, replacing privatenet-us-vm's external IP:
    ```
    ping -c 3 Enter privatenet-us-vm's external IP here
    ```
2. Ping the internal IP addresses
    a. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.
    b. Note the internal IP addresses for mynet-eu-vm, managementnet-us-vm, and privatenet-us-vm.
    c. Return to the SSH terminal for mynet-us-vm.
    d. To test connectivity to mynet-eu-vm's internal IP, run the following command, replacing mynet-eu-vm's internal IP:
    ```
    ping -c 3 Enter mynet-eu-vm's internal IP here
    ```
    e. To test connectivity to managementnet-us-vm's internal IP, run the following command, replacing managementnet-us-vm's internal IP:
    ```
    ping -c 3 Enter managementnet-us-vm's internal IP here
    ```
    f. To test connectivity to privatenet-us-vm's internal IP, run the following command, replacing privatenet-us-vm's internal IP:
    ```
    ping -c 3 Enter privatenet-us-vm's internal IP here
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Create a VM instance with multiple network interfaces</b></summary>
  <br/>
  <p>
    
1. Create the VM instance with multiple network interfaces
    a. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.

    b. Click Create instance.

    c. Set the following values, leave all other values at their defaults:
    
    | Property |	Value (type value or select option as specified) |
    | :---: | :---: |
    | Name |	vm-appliance |
    | Region |	us-central1 |
    | Zone |	us-central1-f |
    | Series |	N1 |
    | Machine type | 4 vCPUs (n1-standard-4) |
    
    d. Click NETWORKING, DISKS, SECURITY, MANAGEMENT, SOLE-TENANCY.

    e. Click Networking.

    f. For Network interfaces, click the dropdown to edit.

    g. Set the following values, leave all other values at their defaults:
    
    | Property |	Value (type value or select option as specified) |
    | :---: | :---: |
    | Network |	privatenet |
    | Subnetwork |	privatesubnet-us |
    
    h. Click Done.
  
    i. Click Add network interface.

    j. Set the following values, leave all other values at their defaults:
    
    | Property |	Value (type value or select option as specified) |
    | :---: | :---: |
    | Network |	managementnet |
    | Subnetwork |	managementsubnet-us |
    
    k. Click Done.

    l. Click Add network interface.

    m. Set the following values, leave all other values at their defaults:
        
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Network | mynetwork |
    | Subnetwork | mynetwork |
    
    n. Click Done. Click Create.
    
2. Explore the network interface details
    
    a. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    
    b. Click nic0 within the Internal IP address of vm-appliance to open the Network interface details page.
    
    c. Verify that nic0 is attached to privatesubnet-us, is assigned an internal IP address within that subnet (172.16.0.0/24), and has applicable firewall rules.
    
    d. Click nic0 and select nic1.
    
    e. Verify that nic1 is attached to managementsubnet-us, is assigned an internal IP address within that subnet (10.130.0.0/20), and has applicable firewall rules.
    
    f. Click nic1 and select nic2.
    
    g. Verify that nic2 is attached to mynetwork, is assigned an internal IP address within that subnet (10.128.0.0/20), and has applicable firewall rules.
    
    h. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.
    
    i. For vm-appliance, click SSH to launch a terminal and connect.
    
    j. Run the following, to list the network interfaces within the VM instance:
    
    ```
    sudo ifconfig
    ```
    
3. Explore the network interface connectivity
    a. In the Cloud Console, navigate to Navigation menu > Compute Engine > VM instances.
    b. Note the internal IP addresses for privatenet-us-vm, managementnet-us-vm, mynet-us-vm, and mynet-eu-vm.
    c. Return to the SSH terminal for vm-appliance.
    d. To test connectivity to privatenet-us-vm's internal IP, run the following command, replacing privatenet-us-vm's internal IP:
    ```
    ping -c 3 Enter privatenet-us-vm's internal IP here
    ``` 
    e. Repeat the same test by running the following:
    ```
    ping -c 3 privatenet-us-vm
    ```
    f. To test connectivity to managementnet-us-vm's internal IP, run the following command, replacing managementnet-us-vm's internal IP:
    ```
    ping -c 3 Enter managementnet-us-vm's internal IP here
    ```
    g. To test connectivity to mynet-us-vm's internal IP, run the following command, replacing mynet-us-vm's internal IP:
    ```
    To test connectivity to mynet-us-vm's internal IP, run the following command, replacing mynet-us-vm's internal IP:
    ```
    h. To test connectivity to mynet-eu-vm's internal IP, run the following command, replacing mynet-eu-vm's internal IP:
    ```
    ping -c 3 Enter mynet-eu-vm's internal IP here
    ```
    
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Which instance(s) should you be able to ping from mynet-us-vm using internal IP addresses?
  - [ ] privatenet-us-vm
  - [X] mynet-eu-vm
  - [ ] managementnet-us-vm



  </p>
</details>
<br/>

## Connect With Us:

**GDSC PDEU:**
- Join Us: https://gdsc.community.dev/pandit-deendayal-petroleum-university/
- YouTube: https://www.youtube.com/channel/UCv4guMYXw_6oc9KSmVlbebA
- GitHub: https://github.com/gdsc-pdeu
- LinkedIn: https://linkedin.com/company/developer-student-clubs-pdeu
- Instagram: https://www.instagram.com/dsc.pdeu/

**GDSC Lead - Jay Gohil:**
- Website: https://jay-gohil.me/
- LinkedIn: https://www.linkedin.com/in/jay--gohil/
- GitHub: https://github.com/gohil-jay
- Instagram: https://www.instagram.com/_jay.gohil/

**GCP Facilitator - Jay Patel:**
- Website: http://pateljay.me/
- LinkedIn: https://www.linkedin.com/in/--jaypatel--/
- GitHub: https://github.com/jaypatel31
- Instagram: https://www.instagram.com/jaypatel98196/
