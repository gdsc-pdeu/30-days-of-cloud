# Quest-2: Perform Foundational Infrastructure Tasks in Google Cloud
## Lab-5: Google Cloud Pub/Sub: Qwik Start - Console

<details> 
  <summary><b>Task 1: Setting up Pub/Sub</b></summary>
  <br/>
  <p>
    
1. Click Navigation menu > Pub/Sub > Topics.
2. Click Create topic.
3. The topic must have a unique name. For this lab, name your topic MyTopic. In the Create a topic dialog:
    a. For Topic ID, type MyTopic.
    b. Leave Encryption at the default value.
    c. Click CREATE TOPIC.

  </p>
</details>
<br/>
  
<details> 
  <summary><b>Task 2: Add a subscription</b></summary>
  <br/>
  <p>

1. Click Topics in the left panel to return to the Topics page. For the topic you just made click the three dot icon > Create subscription.
2. In the Add subscription to topic dialog:
    a. Type a name for the subscription, such as MySub
    b. Set the Delivery Type to Pull.
    c. Leave all other options at the default values.\
    d. Click Create.
    
  </p>
</details>
<br/>

<details> 
  <summary><b>Task 3: Publish a message to the topic</b></summary>
  <br/>
  <p>
    
1. At the top of the Topics details dialog, click PUBLISH MESSAGE.
2. Enter Hello World in the Message field and click Publish.

  </p>
</details>
<br/>

<details> 
  <summary><b>Task 4: View the message</b></summary>
  <br/>
  <p>
    
1. To view the message you'll use the subscription (MySub) to pull the message (Hello World) from the topic (MyTopic).
    - Enter the following command in command line.
    ```
    gcloud pubsub subscriptions pull --auto-ack MySub
    ```
  </p>
</details>
<br/>

<details> 
  <summary><b>Test your knowledge</b></summary>
  <br/>
  <p>
    
- Q. A publisher application creates and sends messages to a ____. Subscriber applications create a ____ to a topic to receive messages from it.
  - [ ] subscription, topic
  - [X] topic, subscription
  - [ ] topic, topic
  - [ ] subscription, subscription
 
- Q. Cloud Pub/Sub is an asynchronous messaging service designed to be highly reliable and scalable.
  - [X] True
  - [ ] False
    

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
