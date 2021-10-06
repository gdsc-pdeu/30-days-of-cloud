# Quest-5: Getting Started: Create and Manage Cloud Resources
## Lab-4: HTTP Load Balancer with Cloud Armor

<details> 
  <summary><b>Task 1: Configure HTTP and health check firewall rules</b></summary>
  <br/>
  <p>
    
1. Create the HTTP firewall rule
    a. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > VPC network > Firewall.
    b. Notice the existing ICMP, internal, RDP, and SSH firewall rules.
    c. Click Create Firewall Rule.
    d. Set the following values, leave all other values at their defaults:
    
    | Property | Value |
    | :---: | :---: |
    | Name | default-allow-http |
    | Network | default |
    | Targets | Specified target tags |
    | Target tags | http-server |
    | Source filter | IP Ranges |
    | Source IP ranges | 	0.0.0.0/0 |
    | Protocols and ports | 	Specified protocols and ports, and then check tcp, type: 80 |
    
    d. Click Create.
    
2. Create the health check firewall rules
    a. Still in the Firewall rules page, click Create Firewall Rule.
    b. Set the following values, leave all other values at their defaults:

    | Property | Value |
    | :---: | :---: |
    | Name | default-allow-health-check |
    | Network | default |
    | Targets | Specified target tags |
    | Target tags | http-server |
    | Source filter | IP Ranges |
    | Source IP ranges | 130.211.0.0/22, 35.191.0.0/16 |
    | Protocols and ports | Specified protocols and ports, and then check tcp. |
    
    c. Click Create.
   
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Configure instance templates and create instance groupse</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > Instance templates, and then click Create instance template.
2. For Name, type us-east1-template.
3. For Series, select N1.
4. Click Management, security, disks, networking, sole tenancy.
5. Click the Management tab.
6. Under Metadata, specify the following:

    | Key | Value |
    | :---: | :---: |
    | startup-script-url | gs://cloud-training/gcpnet/httplb/startup.sh |

7. Click Networking.
8. Set the following values and leave all other values at their defaults:
    
    | Property | Value |
    | :---: | :---: |
    | Network | default |
    | Subnet | default (us-east1) |
    | Network tags | http-server |

9. Click Create.
10. Wait for the instance template to be created.
11. Now create another instance template for subnet-b by copying us-east1-template:

    a. Click on us-east1-template and then click on Create Similar option from the top.
    
    b. For Name, type europe-west1-template.
    
    c. Click Management, security, disks, networking, sole tenancy.
    
    d. Click Networking.
    
    e. For Subnet, select default (europe-west1).
    
    f. Click Create.
 
12. Create the managed instance groups
    a. Still in Compute Engine, click Instance groups in the left menu.
    b. Click Create instance group.
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | us-east1-mig |
    | Location | Multiple zones |
    | Region | us-east1 |
    | Instance template | us-east1-template |
    | Autoscaling > Autoscaling Policy > Click Pencil icon > Metric type | CPU utilization |
    | Target CPU utilization | 80, click Done. |
    | Cool-down period | 45 |
    | Minimum number of instances | 1 |
    | Maximum number of instances | 5 |
    
    d. Click Create.
 
13. Now repeat the same procedure for create a second instance group for europe-west1-mig in europe-west1:
    
    a. Click Create Instance group.
    
    b. Set the following values, leave all other values at their defaults: 
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | europe-west1-mig |
    | Location | Multiple zones |
    | Region | europe-west1 |
    | Instance template | europe-west1-template |
    | Autoscaling > Autoscaling Policy > Click Pencil icon > Metric type | CPU utilization |
    | Target CPU utilization | 80, click Done. |
    | Cool-down period | 45 |
    | Minimum number of instances | 1 |
    | Maximum number of instances | 5 |
    
    c. Click Create.
    
14. Verify the backends
    
    a. Still in Compute Engine, click VM instances in the left menu.
    
    b. Notice the instances that start with us-east1-mig and europe-west1-mig.
    
    c. Click on the External IP of an instance of us-east1-mig.
    
    d. Click on the External IP of an instance of europe-west1-mig.
    

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Configure the HTTP Load Balancer</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, click Navigation menu (mainmenu.png) > click Network Services > Load balancing, and then click Create load balancer.
2. Under HTTP(S) Load Balancing, click on Start configuration.
3. Select From Internet to my VMs, and click Continue.
4. Set the Name to http-lb.
5. Configure the backend
    a. Click on Backend configuration.
    b. For Backend services & backend buckets, click Create a backend service.
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | http-backend |
    | Instance group | us-east1-mig |
    | Port numbers | 80 |
    | Balancing mode | Rate |
    | Maximum RPS | 50 |
    | Capacity | 100 |
    
    d. Click Done.
    e. Click Add backend.
    f. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Instance group | europe-west1-mig |
    | Port numbers | 80 |
    | Balancing mode | Utilization |
    | Maximum backend utilization | 80 |
    | Capacity | 100 |
    
    g. Click Done.
    h. For Health Check, select Create a health check.
    i. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | http-health-check |
    | Protocol | TCP |
    | Port | 80 |
    
    j. Click Save.
    k. Check the Enable Logging box.
    l. Set the Sample Rate to 1:
    m. Click Create to create the backend service.
    
    
6. Configure the frontend
    a. Click on Frontend configuration.
    b. Specify the following, leaving all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Protocol | HTTP |
    | IP version | IPv4 |
    | IP address | Ephemeral |
    | Port | 80 |
    
    c. Click Done.
    d. Click Add Frontend IP and port.
    e. Specify the following, leaving all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Protocol | HTTP |
    | IP version | IPv6 |
    | IP address | Ephemeral |
    | Port | 80 |
    
    f. Click Done.
    
7. Review and create the HTTP Load Balancer
    
    a. Click on Review and finalize.
    
    b. Review the Backend services and Frontend.
    
    c. Click on Create.
    
    d. Wait for the load balancer to be created.
    
    e. Click on the name of the load balancer (http-lb).
    
    f. Note the IPv4 and IPv6 addresses of the load balancer for the next task. They will be referred to as [LB_IP_v4] and [LB_IP_v6], respectively.
    
    
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Test the HTTP Load Balancer</b></summary>
  <br/>
  <p>
    
1. Stress test the HTTP Load Balancer
    a. In the Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    b. Click Create instance.
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | siege-vm |
    | Region | us-west1 |
    | Zone | us-west1-c |
    | Series | N1 |
    
    d. Click Create.
    e. Wait for the siege-vm instance to be created.
    f. For siege-vm, click SSH to launch a terminal and connect.
    g. Run the following command, to install siege:
    ```
    sudo apt-get -y install siege
    ```
    h. To store the IPv4 address of the HTTP Load Balancer in an environment variable, run the following command, replacing [LB_IP_v4] with the IPv4 address:
    ```
    export LB_IP=[LB_IP_v4]
    ```
    i. To simulate a load, run the following command:
    ```
    siege -c 250 http://$LB_IP
    ```
    j. In the Cloud Console, on the Navigation menu (mainmenu.png), click Network Services > Load balancing.
    
    k. Click Backends.
    
    l. Click http-backend.
    
    m. Monitor the Frontend Location (Total inbound traffic) between North America and the two backends for 2 to 3 minutes.
    
    n. Return to the SSH terminal of siege-vm.
    
    o. Press CTRL+C to stop siege.
 
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Denylist the siege-vm</b></summary>
  <br/>
  <p>
    
1. Create the security policy
    
    a. In the Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    
    b. Note the External IP of the siege-vm. This will be referred to as [SIEGE_IP].
    
    c. In the Cloud Console, navigate to Navigation menu > Network Security > Cloud Armor.
    
    d. Click Create policy.
    
    e. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | denylist-siege |
    | Default rule action | Allow |
    
    f. Click Next step.
    
    g. Click Add rule.
    
    h. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Condition | Enter the SIEGE_IP |
    | Action | Deny |
    | Deny status | 403 (Forbidden) |
    | Priority | 1000 |
    
    i. Click Done.
    
    j. Click Next step.
    
    k. Click Add Target.
    
    l. For Type, select Load balancer backend service.
    
    m. For Target, select http-backend.
    
    n. Click Create policy.
    
    o. Wait for the policy to be created before moving to the next step.
    
    
2. Verify the security policy
    
    a. Return to the SSH terminal of siege-vm.
    
    b. To access the load balancer, run the following:
    
    ```
    curl http://$LB_IP
    ```
    c. Open a new tab in your browser and navigate to http://[LB_IP_v4]. Make sure to replace [LB_IP_v4] with the IPv4 address of the load balancer.
    
    d. Back in the SSH terminal of siege-vm, to simulate a load, run the following command:
    
    ```
    siege -c 250 http://$LB_IP
    ```
    e. In the Console, navigate to Navigation menu > Network Security > Cloud Armor.
    
    f. Click denylist-siege.
    
    g. Click Logs.
    
    h. Click View policy logs.
    
    i. On the Logging page, make sure to clear all the text in the Query preview. Select resource to Cloud HTTP Load Balancer > http-lb-forwarding-rule > http-lb then click Add.
    
    j. Now click Run Query.
    
    k. Expand a log entry in Query results.
    
    l. Expand httpRequest.
    
    m. The request should be from the siege-vm IP address. If not, expand another log entry.
    
    n. Expand jsonPayload.
    
    o. Expand enforcedSecurityPolicy.
    
    p. Notice that the configuredAction is to DENY with the name denylist-siege.
 
    
  </p>
</details>
<br/>


<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. The HTTP load balancer should forward traffic to the region that is closest to you.
  - [X] True
  - [ ] False  

- Q. Which of these fields identify the region of the backend?
  - [ ] Client IP
  - [X] Hostname
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
