# Quest-1: Getting Started: Create and Manage Cloud Resources
## Lab-2: Compute Engine: Qwik Start - Windows

<details> 
  <summary><b>Task 1: Create a virtual machine instance</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, on the Navigation menu Navigation menu, click Compute Engine > VM instances, and then click Create Instance.
2. In the Machine configuration section, for Series select **N1**.
3. In the Boot disk section, click Change to begin configuring your boot disk.
4. Choose Windows Server 2012 R2 Datacenter, and then click Select. Leave all other settings as their defaults.
5. Click **Create** to create the instance.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Remote Desktop (RDP) into the Windows Server</b></summary>
  <br/>
  <p>
    
1. Test the status of Windows Startup

   ```
   gcloud compute instances get-serial-port-output instance-1
   ```
   
2. If prompted, type **n** and press **Enter**.
3. Repeat the command until you see the following in the command output, which tells you that the OS components have initialized and the Windows Server is ready to accept your RDP connection (attempt in the next step).

  ```
  ------------------------------------------------------------
  Instance setup finished. instance-1 is ready to use.
  ------------------------------------------------------------
  ```
  
4. RDP into the Windows Server
    - To set a password for logging into the RDP, run the following command in Cloud Shell terminal and replace [instance] with the VM Instance that you have created and set [username] as admin.

    ```
    gcloud compute reset-windows-password [instance] --zone us-central1-a --user [username]
    ```
  
5. If asked Would you like to set or reset the password for [admin] (Y/n)?, enter Y.

    
  </p>
</details>
<br/>


<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. We can create a Windows instance in Google Cloud by changing its ____ in the VM instance console.
  - [ ] API Access
  - [ ] Machine Type
  - [X] Boot disk to Windows image
  - [ ] Firewall rules
 
- Q. Which command is used to check whether the server is ready for an RDP connection?
  - [X] gcloud compute instances get-serial-port-output
  - [ ] gcloud compute instances create
  - [ ] gcloud compute ssh
  - [ ] gcloud compute instances list  
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
