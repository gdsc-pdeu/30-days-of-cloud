# Quest-5: Getting Started: Create and Manage Cloud Resources
## Lab-5: Create an Internal Load Balancer

<details> 
  <summary><b>Task 1: Configure HTTP and health check firewall rules</b></summary>
  <br/>
  <p>
    
1. Explore the my-internal-app network
    a. In the Console, navigate to Navigation menu > VPC network > VPC networks.
    b. Scroll down and notice the my-internal-app network with its subnets: subnet-a and subnet-b
    
2. Create the HTTP firewall rule
    
    a. Still in VPC network, in the left pane click Firewall.
    
    b. Notice the app-allow-icmp and app-allow-ssh-rdp firewall rules.
    
    c. Click Create Firewall Rule.
    
    d. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | app-allow-http |
    | Network | my-internal-app |
    | Targets | Specified target tags |
    | Target tags | lb-backend |
    | Source filter | IP Ranges |
    | Source IP ranges | 0.0.0.0/0 |
    | Protocols and ports | Specified protocols and ports, and then check tcp, type: 80 |
    
    e. Click Create.
   
3. Create the health check firewall rules
    
    a. Still in the Firewall rules page, click Create Firewall Rule.

    b. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | app-allow-health-check |
    | Targets | Specified target tags |
    | Target tags | lb-backend |
    | Source filter | IP Ranges |
    | Source IP ranges | 130.211.0.0/22 35.191.0.0/16 |
    | Protocols and ports | Specified protocols and ports, and then check tcp |
    
    c. Click Create
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Configure instance templates and create instance groups</b></summary>
  <br/>
  <p>
    
1. In the Console, navigate to Navigation menu > Compute Engine > Instance templates.
2. Click Create instance template.
3. For Name, type instance-template-1.
4. For Series, select N1.
5. Click Management, security, disks, networking, sole tenancy.
6. Click Management.
7. Under Metadata, specify the following:

    | Key | Value |
    | :---: | :---: |
    | startup-script-url | gs://cloud-training/gcpnet/ilb/startup.sh |

7. Click Networking.
8. Set the following values and leave all other values at their defaults:
    
    | Property | Value |
    | :---: | :---: |
    | Network | my-internal-app |
    | Subnet | subnet-a |
    | Network tags | lb-backend |

9. Click Create.
10. Wait for the instance template to be created.
11. Now create another instance template for subnet-b by copying us-east1-template:

    a. Still in Instance templates, check the box next to instance-template-1, then click Copy. You will see the instance is named instance-template-2.
    
    b. Click Management, security, disks, networking, sole tenancy.
    
    c. Click the Networking tab.

    d. Select subnet-b as the Subnetwork.

    e. Click Create.
 
12. Create the managed instance groups
    a. Still in Compute Engine, click Instance groups in the left menu.
    b. Click Create instance group.
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | instance-group-1 |
    | Location | Single-zone |
    | Region | us-east1 |
    | Zone | us-east1-a |
    | Instance template | instance-template-1 |
    | Autoscaling > Autoscaling Policy > Click Pencil icon > Metric type | CPU utilization |
    | Target CPU utilization | 80, click Done. |
    | Cool-down period | 45 |
    | Minimum number of instances | 1 |
    | Maximum number of instances | 5 |
    
    d. Click Create.
 
13. Repeat the same procedure for instance-group-2 in us-central1-b:
    a. Click Create Instance group.
    b. Set the following values, leave all other values at their defaults: 
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | instance-group-1 |
    | Location | Single-zone |
    | Region | us-east1 |
    | Zone | us-east1-b |
    | Instance template | instance-template-2 |
    | Autoscaling > Autoscaling Policy > Click Pencil icon > Metric type | CPU utilization |
    | Target CPU utilization | 80, click Done. |
    | Cool-down period | 45 |
    | Minimum number of instances | 1 |
    | Maximum number of instances | 5 |
    
    c. Click Create.
    
14. Verify the backends
    a. Still in Compute Engine, click VM instances in the left menu.
    
    b. Notice two instances that start with instance-group-1 and instance-group-2
    
    c. Click Create instance.

    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | utility-vm |
    | Region | us-central1 |
    | Zone | us-central1-f |
    | Series | N1 |
    | Machine type | f1-micro (1 shared vCPU) |
    
    d. Click Management, security, disks, networking, sole tenancy.

    e. Click Networking.

    f. For Network interfaces, click the pencil icon to edit.

    g. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Network | my-internal-app |
    | Subnetwork | subnet-a |
    | Primary internal IP | Ephemeral (Custom) |
    | Custom ephemeral IP | address	10.10.20.50 |
    
    h. Click Create.
    
    i. For utility-vm, click SSH to launch a terminal and connect.

    j. To verify the welcome page for instance-group-1-xxxx, run the following command:
    ```
    curl 10.10.20.2
    ```
    k. To verify the welcome page for instance-group-2-xxxx, run the following command:
    ```
    curl 10.10.30.2
    ```
    l. Close the SSH terminal to utility-vm:
    ```
    exit
    ```
    

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Configure the Internal Load Balancer</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, navigate to Navigation menu > Network Services > Load balancing, and then click Create load balancer.
2. Under TCP Load Balancing, click on Start configuration.
3. For Internet facing or internal only, select Only between my VMs.
4. Click Continue.
5. For Name, type my-ilb.
5. Configure the backend
    
    a. Click on Backend configuration.
    
    b. For Backend services & backend buckets, click Create a backend service.
    
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Region | us-central1 |
    | Network | my-internal-app |
    | Instance group | instance-group-1 (us-central1-a) |
    
    d. Click Add backend.
    
    e. For Instance group, select instance-group-2 (us-central1-b).
    
    f. For Health Check, select Create a health check.
    
    g. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | my-ilb-health-check |
    | Protocol | TCP |
    | Port | 80 |
    
    g. Click Save and Continue.

    h. Verify that there is a blue check mark next to Backend configuration in the Cloud Console. If not, double-check that you have completed all the steps above
    
6. Configure the frontend
    a. Click on Frontend configuration.
    b. Specify the following, leaving all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Subnetwork | subnet-b |
    | Internal IP | Reserve a static internal IP address |
    
    c. Specify the following, leaving all other values with their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | my-ilb-ip |
    | Static IP address | Let me choose |
    | Custom IP address | 10.10.30.5 |
    
    f. Click Reserve.

    g. For Ports, type 80.

    h. Click Done.
    
7. Review and create the Internal Load Balancer
    a. Click on Review and finalize.
    b. Review the Backend services and Frontend.
    c. Click on Create. Wait for the Load Balancer to be created, before moving to the next task.
    
    
  </p>
</details>
<br/>


<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Which of these fields identify the location of the backend?
  - [ ] Client IP
  - [ ] Hostname
  - [X] Server Location

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
