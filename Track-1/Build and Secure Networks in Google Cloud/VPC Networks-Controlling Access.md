# Quest-5: Getting Started: Create and Manage Cloud Resources
## Lab-3: VPC Networks - Controlling Access

<details> 
  <summary><b>Task 1: Create the web servers</b></summary>
  <br/>
  <p>
    
1. Create the blue server
    a. In the Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    b. Click Create Instance.
    c. Set the following values, leave all other values at their defaults:
    
    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | blue |
    | Region | us-central1 (Iowa) |
    | Zone | us-central1-a |
    
    d. Click Management, disks, networking, sole tenancy.

    e. Click Networking.
    f. For Network tags, type web-server.
    g. Click Create.
    
2. Create the green server
    a. Still in the Console, in the VM instances dialog, click Create instance.

    b. Set the following values, leave all other values at their defaults:

    | Property | Value (type value or select option as specified) |
    | :---: | :---: |
    | Name | green |
    | Region | us-central1 (Iowa) |
    | Zone | us-central1-a |
    
    c. Click Create.
    
3. Install nginx and customize the welcome page
    a. Still in the VM instances dialog, for blue, click SSH to launch a terminal and connect.
    b. In the SSH terminal to blue, run the following command to install nginx:
    ```
    sudo apt-get install nginx-light -y
    ```
    c. Open the welcome page in the nano editor:
    ```
    sudo nano /var/www/html/index.nginx-debian.html
    ```
    d. Replace the <h1>Welcome to nginx!</h1> line with <h1>Welcome to the blue server!</h1>.
    e. Press CTRL+o, ENTER, CTRL+x.
    f. Verify the change:
    ```
    cat /var/www/html/index.nginx-debian.html
    ```
    g. Close the SSH terminal to blue:
    h. For green, click SSH to launch a terminal and connect.
    i. Install nginx:
    ```
    sudo apt-get install nginx-light -y
    ```
    j. Open the welcome page in the nano editor:
    ```
    sudo nano /var/www/html/index.nginx-debian.html
    ```
    k. Replace the <h1>Welcome to nginx!</h1> line with <h1>Welcome to the green server!</h1>. 
    l. Press CTRL+o, ENTER, CTRL+x.
    m. Verify the change:
    ```
    cat /var/www/html/index.nginx-debian.html
    ```
    n. Close the SSH terminal to green:
    
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Create the firewall rule</b></summary>
  <br/>
  <p>
    
1. In the Console, navigate to Navigation menu (mainmenu.png) > VPC network > Firewall.
2. Notice the default-allow-internal firewall rule.
3. Click Create Firewall Rule.
4. Set the following values, leave all other values at their defaults and click Create:

    | Property | Value |
    | :---: | :---: |
    | Name | allow-http-web-server |
    | Network | default |
    | Targets | Specified target tags |
    | Target tags | web-server |
    | Source filter | IP Ranges |
    | Source IP ranges | 	0.0.0.0/0 |
    | Protocols and ports | Specified protocols and ports, and then check tcp, type: 80; and check Other protocols, type: icmp. |
 
5. Click Create.

6. Create a test-vm
    ```
    gcloud compute instances create test-vm --machine-type=f1-micro --subnet=default --zone=us-central1-a
    ```

7. Test HTTP connectivity
    a. In the Console, navigate to Navigation menu (mainmenu.png) > Compute Engine > VM instances.
    b. Note the internal and external IP addresses of blue and green.
    c. For test-vm, click SSH to launch a terminal and connect.
    d. To test HTTP connectivity to blue's internal IP, run the following command, replacing blue's internal IP:
    ```
    curl Enter blue's internal IP here
    ```
    e. To test HTTP connectivity to green's internal IP, run the following command, replacing green's internal IP:
    ```
    curl -c 3 Enter green's internal IP here
    ```
    f. To test HTTP connectivity to blue's external IP, run the following command, replacing blue's external IP:
    ```
    curl Enter blue's external IP here
    ```
    g. To test HTTP connectivity to green's external IP, run the following command, replacing green's external IP:
    ```
    curl -c 3 Enter green's external IP here
    ```
    h. Press CTRL+c to stop the HTTP request.
  
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Explore the Network and Security Admin roles</b></summary>
  <br/>
  <p>
    
1. Return to the SSH terminal of the test-vm instance.
2. Try to list the available firewall rules:
    ```
    gcloud compute firewall-rules list
    ```
3. Try to delete the allow-http-web-server firewall rule:
    ```
    gcloud compute firewall-rules delete allow-http-web-server
    ```
4. Enter Y, if asked to continue.
5. Create a service account
    a. In the Console, navigate to Navigation menu (mainmenu.png) > IAM & admin > Service Accounts.
    b. Notice the Compute Engine default service account.
    c. Click Create service account.
    d. Set the Service account name to Network-admin and click CREATE AND CONTINUE.
    e. For Select a role, select Compute Engine > Compute Network Admin and click CONTINUE then click DONE.
    f. After creating service account 'Network-admin', click on three dots at the right corner and click Manage Key in the dropdown, then click on Add Key and select Create new key from the dropdown. Click Create to download your JSON output.
    g. Click Close.
    h. A JSON key file download to your local computer. Find this key file, you will upload it in to the VM in a later step.
    i. Rename the JSON key file on your local machine to credentials.json
6. Authorize test-vm to use the Network-admin service account.
    a. Return to the SSH terminal of the test-vm instance.
    b. To upload credentials.json through the SSH VM terminal, click on the gear icon in the upper-right corner, and then click Upload file.
    c. Select credentials.json and upload it.
    d. Click Close in the File Transfer window.
    e. Authorize the VM with the credentials you just uploaded:
    ```
    gcloud auth activate-service-account --key-file credentials.json
    ```
    f. Try to list the available firewall rules:
    ```
    gcloud compute firewall-rules list
    ```
    g. Try to delete the allow-http-web-server firewall rule:
    ```
    gcloud compute firewall-rules delete allow-http-web-server
    ```
    h. Enter Y, if asked to continue.
7. Update service account and verify permissions
    a. In the Console, navigate to Navigation menu (mainmenu.png) > IAM & admin > IAM.
    b. Find the Network-admin account. Focus on the Name column to identify this account.
    c. Click on the pencil icon for the Network-admin account.
    d. Change Role to Compute Engine > Compute Security Admin.
    e. Click Save.
    f. Return to the SSH terminal of the test-vm instance.
    g. Try to list the available firewall rules:
    ```
    gcloud compute firewall-rules list
    ```
    h. Try to delete the allow-http-web-server firewall rule:
    ```
    gcloud compute firewall-rules delete allow-http-web-server
    ```
8. Verify the deletion of the firewall rule
    a. Return to the SSH terminal of the test-vm instance.
    b. To test HTTP connectivity to blue's external IP, run the following command, replacing blue's external IP:
    ```
    curl -c 3 Enter blue's external IP here
    ```
    c. Press CTRL+c to stop the HTTP request.
    
  </p>
</details>
<br/>


<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. The Security Admin role, provides permissions to:
  - [X] List the available firewall rules
  - [ ] Neither list, create, modify, or delete the available firewall rules
  - [X] Delete the available firewall rules
  - [ ] Create a firewall rules
  - [X] Modify the available firewall rules

- Q. The Network Admin role, provides permissions to:
  - [X] List the available firewall rules
  - [ ] Neither list, create, modify, or delete the available firewall rules
  - [ ] Delete the available firewall rules
  - [ ] Create a firewall rules
  - [ ] Modify the available firewall rules 
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
