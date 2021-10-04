# Quest-1: Getting Started: Create and Manage Cloud Resources
## Lab-3: Getting Started with Cloud Shell and gcloud

<details> 
  <summary><b>Task 1: Configure your environment</b></summary>
  <br/>
  <p>
    
1. Copy your project ID to your clipboard or text editor. The project ID is listed in 2 places:
   - In the Google Cloud Console, on the Dashboard, under Project info. (Click Navigation menu (Navigation menu), and then click Home > Dashboard.)
   - On the Qwiklabs tab near your username and password.
2. In Cloud Shell, run the following gcloud command, replacing <your_project_ID> with the project ID you copied:
    ```
    gcloud compute project-info describe --project <your_project_ID>
    ```
3. Set environment variables
    a. Create an environment variable to store your Project ID, replacing <your_project_ID> with the value for name from the gcloud compute project-info describe command you ran earlier:
    ```
    export PROJECT_ID=<your_project_ID>
    ```
    b. Create an environment variable to store your Zone, replacing <your_zone> with the value for zone from the gcloud compute project-info describe command you ran earlier:
    ```
    export ZONE=<your_zone>
    ```
    c. To verify that your variables were set properly, run the following commands:
    ```
    echo $PROJECT_ID
    echo $ZONE
    ```
    
4. Create a virtual machine with the gcloud tool
    a. To create your VM, run the following command:
    ```
    gcloud compute instances create gcelab2 --machine-type n1-standard-2 --zone $ZONE
    ```
    b. To open help for the create command, run the following command:
    ```
    gcloud compute instances create --help
    ```
    c.  Press ENTER or the spacebar to scroll through the help content. To exit the content, type Q.

    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Install a new component</b></summary>
  <br/>
  <p>
    
1. Install the beta components:

   ```
   sudo apt-get install google-cloud-sdk
   ```
   
2. Enable the gcloud interactive mode:
    ```
    gcloud beta interactive
    ```
3. To try this feature, start typing the following command, and use auto-complete to replace <your_vm> with an existing VM in your project:
    ```
    gcloud compute instances describe <your_vm>
    ```
  
4. To exit from the interactive mode, run the following command:
    ```
    exit
    ```
    
  </p>
</details>
<br/>
    
<details> 
  <summary><b>Task 3: Connect to your VM instance with SSH</b></summary>
  <br/>
  <p>
    
1. To connect to your VM with SSH, run the following command:

   ```
   gcloud compute ssh gcelab2 --zone $ZONE
   ```
   
2. To continue, type Y.
    ```
    Generating public/private rsa key pair.
    Enter passphrase (empty for no passphrase)
    ```
3. To leave the passphrase empty, press ENTER twice.
    ```
    gcloud compute instances describe <your_vm>
    ```
  
4. You don't need to do anything here, so to disconnect from SSH and exit the remote shell, run the following command:
    ```
    exit
    ```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Use the Home directory</b></summary>
  <br/>
  <p>
    
1. Change your current working directory:

   ```
   cd $HOME
   ```
   
2. Open your .bashrc configuration file by using the vi text editor:
    ```
    vi ./.bashrc
    ```
3. To exit the editor, press ESC, then type :wq, and then press Enter.

  </p>
</details>
<br/>
    
<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. Three basic ways to interact with Google Cloud services and resources are:
  - [X] Client libraries
  - [ ] GStreamer
  - [X] Cloud Console
  - [ ] GLib
  - [X] Command-line interface

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
