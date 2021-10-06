# Quest-4: Deploy and Manage Cloud Environments with Google Cloud
## Lab-4: Troubleshooting Workloads on GKE for Site Reliability Engineers

<details> 
  <summary><b>Task 1: Navigating Google Kubernetes Engine (GKE) Resource Pages</b></summary>
  <br/>
  <p>

1. In Cloud Console, from the Navigation menu go to Kubernetes Engine > Clusters.
2. Confirm that you see two Kubernetes clusters available, cloud-ops-sandbox and loadgenerator. Validate that each cluster has a green checkbox next to it to indicate it is up and running.
3. Click on the cloud-ops-sandbox link under the Name column to navigate to the cluster's Details tab.

4. Click on the Nodes tab to see all the nodes in the cluster. Validate that there is a single node pool.
5. Under the Nodes section of the Nodes tab, click on the link for the first node in the table under the Name column to view more details about the node.
6. On the node details page, note the metrics of the node that are available. These should be listed under the Summary tab and include CPU and Memory Usage as well as Disk I/O among others. This lab generates load using the loadgenerator cluster and you should see metrics activity but no obvious spikes or metrics above the "requested" limit for the graphs displayed on the Summary tab.
7. To investigate further, rather than navigating to each individual node to view its metrics, click on the three dots in the top right corner of the CPU tile and select View in Metrics Explorer.
8. Remove the filter for the nodename by expanding the filter and clicking the delete icon.
9. Under the How do you want to view that data section, set Group by to node_name.    
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Accessing Operational Data Through GKE Dashboards</b></summary>
  <br/>
  <p>
    
1. Navigate to Cloud Monitoring from Cloud Console, from the Navigation Menu go to Monitoring > Dashboards.
2. When on the Dashboards landing page select GKE.
3. Add the following filters to your GKE Dashboard. Click on the Add Filter button at the top of the GKE Dashboard page
4. From the available filters, select Workloads > recommendationservice.
5. Under the Workloads section, click on the recommendationservice to reveal the Deployment details pane. This view presents details on Alerts, Service Level Objectives (SLOs), Events, Metrics and Logs. At this point in the lab, no SLOs are present. You will add an SLO here in the next part of this lab.
6. Click on the Metrics tab to view metrics related to the recommendationservice. You can change the Metrics drop down selection to alter the visualization data provided and view different metrics available for this service.
7. Click on the Logs tab to view logs related to the recommendationservice. You can filter the available logs by using the Severity drop down corresponding to the log level of the entries available. This is useful in an SRE context to find errors recorded in the logs and leverage the entries to troubleshoot issues.
8. Set the Severity to Error in order to filter the recommendationservice logs.
9. At this point, the error related to the problematic code should be obvious. Look for the phrase invalid literal for int() with base 10: '5.0' in the items in the result set. This error appearing in the recommendationservice filters confirms that the service has a bug in the code.
10. In Cloud Shell run the following command:
    ```
    git clone --depth 1 --branch develop https://github.com/GoogleCloudPlatform/cloud-ops-sandbox.git
    ```
    ```
    cd cloud-ops-sandbox/kubernetes-manifests
    ```
11. Navigate to Navigation Menu > Kubernetes Engine > Clusters. Select the three dots to the right of the cloud-ops-sandbox cluster and select the option to Connect.
12. On the Connect to the cluster modal dialog, click the RUN IN CLOUD SHELL button. Press Enter to run the command once populated in Cloud Shell.
13. Last, run the apply command to update the service:    
    ```
    kubectl apply -f recommendationservice.yaml
    ```
  
  
  
  
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Proactive Monitoring with Logs-Based Metrics</b></summary>
  <br/>
  <p>
    
1. From Cloud Console, click on the Navigation Menu > Logging > Logs Explorer.
2. In the Query results section click on the Actions drop down menu and select Create metric. This will open a new tab to create a logs based metric.
3. Enter the following options on the Create logs metric page:
    - Metric Type: Counter
    - Log metric name: Error_Rate_SLI
    - Filter Selection: (Copy and paste the filter below
4. Click Create Metric.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: Creating a Service Level Objective (SLO)</b></summary>
  <br/>
  <p>
    
1. Navigate to Navigation menu > Monitoring > Services. The resulting page will display a list of all services deployed to GKE for the application workload.

2. Select the recommendationservice service from the list of available services which will take you to the Service details page.

3. Click on + Create SLO on the top right of the page.
4. On Step 1 you will be presented with a dialog for creating a new SLO. Set the following parameters:
    - Choose a metric: Availability
    - Request-based or windows-based: Request Based
5. Click Continue.

6. On Step 2 Define SLI details, the Performance Metric will already be assigned to measure the availability of the service based on the percentage of successful requests.
7. Click on Continue.

8. On Step 3 Set your service-level objective (SLO), the user needs to define two additional items: the Compliance period and Performance goal. Make the following selections:

    - Period type: Calendar
    - Period length: Calendar month
    - Performance Goal: 99%

9. Click on Continue.

10. Click Create SLO on the last step of the wizard to complete the SLO creation process.

11. Click on the entry listed and select the Error budget tab once expanded.  
  
  
  
  
  
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: Define an Alert on the Service Level Objective (SLO)</b></summary>
  <br/>
  <p>
    
1. Navigate to Navigation menu > Monitoring > Services.
2. Click on the recommendationservice service from the list of services available.
3. Under the section Current status of 1 SLO, you should see the Service Level Objective created in the last task. Expand the SLO listed.
4. Click the Create SLO Alert button present on the SLO. This will allow you to define an Alert policy when the SLO is violated.
5. Leave the default values:
    - Lookback duration: 60 minutes
    - Burn rate threshold: 10
6. Click Next.
7. On Step 2, you can define a notification channel to receive the alert when the violation is observed. For the purposes of this lab, you can optionally supply an email address or SMS channel to receive a notification.
8. Click Next. Click Save    
    
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
