# Quest-5: Getting Started: Create and Manage Cloud Resources
## Lab-6: Build and Secure Networks in Google Cloud: Challenge Lab

<details> 
  <summary><b>Task 1: Check the firewall rules. Remove the overly permissive rules</b></summary>
  <br/>
  <p>
    
   
```
gcloud compute firewall-rules delete open-access
````
    
  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Navigate to Compute Engine in the Cloud Console and identify the bastion host. The instance should be stopped. Start the instance</b></summary>
  <br/>
  <p>
    
```
Go to Compute Engine > VM Instances > Select bastion > click on Start
```
 

    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: The bastion host is the one machine authorized to receive external SSH traffic. Create a firewall rule that allows SSH (tcp/22) from the IAP service. The firewall rule should be enabled on bastion via a network tag.</b></summary>
  <br/>
  <p>

```
gcloud compute firewall-rules create ssh-ingress --allow=tcp:22 --source-ranges 35.235.240.0/20 --target-tags ssh-ingress --network acme-vpc
```
```
gcloud compute instances add-tags bastion --tags=ssh-ingress --zone=us-central1-b
```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: The juice-shop server serves HTTP traffic. Create a firewall rule that allows traffic on HTTP (tcp/80) to any address. The firewall rule should be enabled on juice-shop via a network tag</b></summary>
  <br/>
  <p>

```
gcloud compute firewall-rules create http-ingress --allow=tcp:80 --source-ranges 0.0.0.0/0 --target-tags http-ingress --network acme-vpc
```
```
gcloud compute instances add-tags juice-shop --tags=http-ingress --zone=us-central1-b
```
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 5: You need to connect to juice-shop from the bastion using SSH. Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address. The firewall rule should be enabled on juice-shop via a network tag</b></summary>
  <br/>
  <p>

```
gcloud compute firewall-rules create internal-ssh-ingress --allow=tcp:22 --source-ranges 192.168.10.0/24 --target-tags internal-ssh-ingress --network acme-vpc
```
```
gcloud compute instances add-tags juice-shop --tags=internal-ssh-ingress --zone=us-central1-b
```
    
  </p>
</details>
<br/>


<details> 
  <summary><b>Task 6: In the Compute Engine instances page, click the SSH button for the bastion host. Once connected, SSH to juice-shop</b></summary>
  <br/>
  <p>

```
ssh <internal IP of the juice-shop>
```
    
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
