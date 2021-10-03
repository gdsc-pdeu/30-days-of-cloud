# Quest-1: Getting Started: Create and Manage Cloud Resources
## Lab-2: Creating a Virtual Machine

<details> 
  <summary><b>Task 1: Create a new instance from the Cloud Console</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, on the Navigation menu, click Compute Engine > VM Instances.
2. To create a new instance, click **CREATE INSTANCE**.
3. There are many parameters you can configure when creating a **new instance**. Use the following for this lab:

  | Field | Value  |
  | :---:   | :-: |
  | Name | gcelab |
  | Region | us-central1 (Iowa) |
  | Zone	| us-central1-f |
  | Series | N1 |
  | Machine Type | 2 vCPU |
  | Boot Disk | New 10 GB balanced persistent disk OS Image: Debian GNU/Linux 10 (buster) |
  | Firewall | Allow HTTP traffic |

4. Click Create.
5. To use SSH to connect to the virtual machine, in the row for your machine, click **SSH**.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Install an NGINX web server</b></summary>
  <br/>
  <p>
    
1. In the SSH terminal, to get root access, run the following command:

   ```
   sudo su -
   ```
   
2. As the root user, update your OS:

  ```
  apt-get update
  ```
  
3. Install NGINX:

  ```
  apt-get install nginx -y
  ```
  
4. Confirm that NGINX is running:

  ```
  ps auwx | grep nginx
  ```
  
5. To see the web page, return to the Cloud Console and click the External IP link in the row for your machine, or add the **External IP** value to **http://EXTERNAL_IP/** in a new browser window or tab.

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create a new instance with gcloud</b></summary>
  <br/>
  <p>
    
1. In the Cloud Shell, use gcloud to create a new virtual machine instance from the command line:

  ```
  gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone us-central1-f
  ```

2. To see all the defaults, run:

  ```
  gcloud compute instances create --help
  ```
  
3. To exit help, press **CTRL + C**.
4. In the Cloud Console, on the Navigation menu, click Compute Engine > VM instances. Your 2 new instances should be listed.
5. You can also use SSH to connect to your instance via gcloud. Make sure to add your zone, or omit the --zone flag if you've set the option globally:

  ```
  gcloud compute ssh gcelab2 --zone us-central1-f
  ```
  
6. Type **Y** to continue.
7. Press ENTER through the passphrase section to leave the passphrase empty.
8. After connecting, disconnect from SSH by exiting from the remote shell:

  ```
  exit
  ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Through which of the following ways can you create a VM instance in Compute Engine?
  - [X] The Cloud Console. 
  - [X] The gcloud command line tool.
    
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
