# Quest-2: Perform Foundational Infrastructure Tasks in Google Cloud
## Lab-3: Cloud Monitoring: Qwik Start

<details> 
  <summary><b>Task 1: Create a Compute Engine instance</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console dashboard, go to Navigation menu > Compute Engine > VM instances, then click Create instance.
2. Fill in the fields as follows, leaving all other fields at the default value:
    | Field |	Value |
    | :---: | :---:|
    | Name | lamp-1-vm |
    | Region | us-central1 (Iowa) |
    | Zone | us-central1-a |
    | Series | N1 |
    | Machine type | n1-standard-2 |
    | Firewall | check Allow HTTP traffic |
    
3. Click Create.
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Add Apache2 HTTP Server to your instance</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, click SSH to open a terminal to your instance.   
2. Run the following commands in the SSH window to set up Apache2 HTTP Server:
    ```
    sudo apt-get update
    ```
    ```
    sudo apt-get install apache2 php7.0
    ```
    - When asked if you want to continue, enter Y.
    ```
    sudo service apache2 restart
    ```
3. Return to the Cloud Console, on the VM instances page. Click the External IP for lamp-1-vm instance to see the Apache2 default page for this instance.
4. Create a Monitoring workspace
    a. In the Cloud Console, click Navigation menu > Monitoring.
    b. Wait for your workspace to be provisioned.
5. Install agents on the VM:
    a. Run the Monitoring agent install script command in the SSH terminal of your VM instance to install the Cloud Monitoring agent.
    ```
    curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh 
    sudo bash add-monitoring-agent-repo.sh
    ```
    ```
    sudo apt-get update
    ```
    ```
    sudo apt-get install stackdriver-agent
    ```
    b. When asked if you want to continue, enter Y.
    c. Run the Logging agent install script command in the SSH terminal of your VM instance to install the Cloud Logging agent
    ```
    curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
    sudo bash add-logging-agent-repo.sh
    ```
    sudo apt-get update
    ```
    ```
    sudo apt-get install google-fluentd
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Create an uptime check</b></summary>
  <br/>
  <p>
    
1. In the Cloud Console, in the left menu, click Uptime checks, and then click Create Uptime Check.
2. Set the following fields:
    - Title: Lamp Uptime Check, then click Next.
    - Protocol: HTTP
    - Resource Type: Instance
    - Applies to: Single, lamp-1-vm
    - Path: leave at default
    - Check Frequency: 1 min
3. Click on Next to leave the other details to default and click Test to verify that your uptime check can connect to the resource.
4. When you see a green check mark everything can connect. Click Create.
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Create an alerting policy</b></summary>
  <br/>
  <p>
    
1. In the left menu, click Alerting, and then click Create Policy.
2. Click Add Condition.
    a. Target: Start typing "VM" in the resource type and metric field, and then select:
    b. Resource Type: VM Instance (gce_instance)
    c. Metric: Type "network", and then select Network traffic (gce_instance+1). Be sure to choose the Network traffic resource with agent.googleapis.com/interface/traffic:
    d. Configuration
    e. Condition: is above
    f. Threshold: 500
    g. For: 1 minute
    h. Click Add
3. Click on Next.
4. Click on drop down arrow next to Notification Channels, then click on Manage Notification Channels.
5. Scroll down the page and click on ADD NEW for Email.
6. In Create Email Channel dialog box, enter your personal email address in the Email Address field and a Display name.
7. Click on Save.
8. Go back to the previous Create alerting policy tab.
9. Click on Notification Channels again, then click on the Refresh icon to get the display name you mentioned in the previous step.
10. Now, select your Display name and click OK.
11. Click Next.
12. Mention the Alert name as Inbound Traffic Alert.
13. Add a message in documentation, which will be included in the emailed alert.
14. Click on Save.
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Create a dashboard and chart</b></summary>
  <br/>
  <p>
    
1. In the left menu select Dashboards, and then Create Dashboard.
2. Name the dashboard Cloud Monitoring LAMP Qwik Start Dashboard.
3. Add the first chart
    a. Click Line option in Chart library.
    b. Name the chart title CPU Load.
    c. Set the Resource type to VM Instance.
    d. Set the Metric CPU load (1m) (You may need to uncheck the only show active box). Refresh the tab to view the graph.
4. Add the second chart
    a. Click + Add Chart and select Line option in Chart library.
    b. Name this chart Received Packets.
    c. Set the resource type to VM Instance.
    d. Set the Metric Received packets (gce_instance). Refresh the tab to view the graph.
    e. Leave the other fields at their default values. You see the chart data.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: View your logs</b></summary>
  <br/>
  <p>
    
1. Select Navigation menu > Logging > Logs Explorer.
2. Select the logs you want to see, in this case, you select the logs for the lamp-1-vm instance you created at the start of this lab:
    a. Click on Resource.
    b. Select VM Instance > lamp-1-vm in the Resource drop-down menu.
    c. Click Add.
    d.Leave the other fields with their default values.
    e. Click the Stream logs.
3. Check out what happens when you start and stop the VM instance.
    a. Open the Compute Engine window in a new browser window. Select Navigation menu > Compute Engine, right-click VM instances > Open link in new window.
    b. Move the Logs Viewer browser window next to the Compute Engine window. This makes it easier to view how changes to the VM are reflected in the logs.
    c. In the Compute Engine window, select the lamp-1-vm instance, click Stop at the top of the screen, and then confirm to stop the instance.
    d. Watch in the Logs View tab for when the VM is stopped.
    e. In the VM instance details window, click Start at the top of the screen, and then confirm. It will take a few minutes for the instance to re-start. Watch the log messages to monitor the start up.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 6: Check the uptime check results and triggered alerts</b></summary>
  <br/>
  <p>
    
1. In the Cloud Logging window, select Navigation menu > Monitoring > Uptime checks. This view provides a list of all active uptime checks, and the status of each in different locations.
    - You will see Lamp Uptime Check listed. Since you have just restarted your instance, the regions are in a failed status. It may take up to 5 minutes for the regions to become active. Reload your browser window as necessary until the regions are active.
2. Click the name of the uptime check, Lamp Uptime Check.
    - Since you have just restarted your instance, it may take some minutes for the regions to become active. Reload your browser window as necessary.
3. Check if alerts have been triggered
    - In the left menu, click Alerting.
    - You see incidents and events listed in the Alerting window.
    - Check your email account. You should see Cloud Monitoring Alerts.
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
