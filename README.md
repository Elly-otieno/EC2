# Amazon EC2

<br>

  **Amazon Elastic Compute Cloud (Amazon EC2)** is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.
  
  Amazon EC2's simple web service interface allows us to obtain and configure capacity with minimal friction. It provides us with complete control of our computing resources and lets us run on Amazon's proven computing environment. Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing us to quickly scale capacity, both up and down, as our computing requirements change.
  
  Amazon EC2 changes the economics of computing by allowing us to pay only for capacity that we actually use. Amazon EC2 provides developers the tools to build failure resilient applications and isolate themselves from common failure scenarios.

## **Objectives**

<br>

- Launch a web server with termination protection enabled

- Monitor our EC2 instance

- Modify the security group that our web server is using to allow HTTP access

- Resize our Amazon EC2 instance to scale

- Test termination protection

- Terminate our EC2 instance

## **Launching our EC2 instance.**

<br>

  Here, we will launch an Amazon EC2 instance with termination protection. Termination protection prevents us from accidentally terminating an EC2 instance. We will deploy our instance with a User Data script that will allow us to deploy a simple web server.

1. In the AWS Management Console on the **Services** menu, choose **EC2**.

2. In the left navigation pane, choose **EC2 Dashboard** to ensure that we are on the dashboard page.

3. Choose **Launch instance**, and then select **Launch instance**.

## **Naming our EC2 instance.**

<br>

  When we name our instance, AWS creates a key value pair. The key for this pair is **Name**, and the value is the name we enter for our EC2 instance.

4. In the **Name and tags** pane, in the **Name** text box, enter `Web Server`.

## **Choosing an Amazon Machine Image (AMI).**

<br>

  An AMI provides the information required to launch an instance, which is a virtual server in the cloud. An AMI includes the following:

    - A template for the root volume for the instance (for example, an operating system or an application server with applications)
    
    - Launch permissions that control which AWS accounts can use the AMI to launch instances
    
    - A block device mapping that specifies the volumes to attach to the instance when it is launched

  The **Quick Start** list contains the most commonly used AMIs. You can also create our own AMI or select an AMI from the AWS Marketplace, an online store where you can sell or buy software that runs on AWS.

5. Locate the **Application and OS Images (Amazon Machine Image)** pane.

6. Under **AMI Machine Image (AMI)**, notice that the **Amazon Linux 2 AMI** image is selected by default. Keep this setting.

## **Choosing an instance type.**

<br>

  Amazon EC2 provides a wide selection of _instance types_ optimized to fit different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give us the flexibility to choose the appropriate mix of resources for our applications. Each instance type includes one or more instance sizes so that we can scale our resources to the requirements of our target workload.
  
  Select a **t3.micro** instance. This instance type has 2 virtual CPU and 1 GiB of memory.

7. From the dropdown, select **t3.micro**.

   **NOTE**: We will be using this instance and any other free instance types for all our demos.

## **Configuring a key pair.**

<br>

  Amazon EC2 uses public–key cryptography to encrypt and decrypt login information. To log in to our instance, we must create a key pair, specify the name of the key pair when we launch the instance, and provide the private key when we connect to the instance.
  
  In this demo, we do not log in to our instance, so we do not require a key pair.

8. In the **Key pair (login)** pane, select **Proceed without a key pair (Not recommended)**.

## **Configuring the network settings.**

<br>

  We use this pane to configure networking settings.
  
  The VPC indicates which virtual private cloud (VPC) we want to launch the instance into. We can have multiple VPCs, including different ones for development, testing, and production. For this demo we will be using preconfigured VPC i.e Lab VPC, for more information on how to create your own VPC [click here](vpc.com)

9. In the **Network settings** pane, choose **Edit**

10. For **VPC - required**, select **Lab VPC**.

11. Still in the **Network settings** pane, configure the Security Group as follows:

      - **Security group name - required**: `Web Server security group`
  
      - D**escription**: `Security group for my web server`

  A _security group_ acts as a virtual firewall that controls the traffic for one or more instances. When launching an instance, we associate one or more security groups with the instance. We add rules to each security group that allow traffic to or from its associated instances. We can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group.

12. Under **Inbound security groups rules** select **Remove**

  In this demo, we will not log into our instance using SSH. Removing SSH access will improve the security of the instance.

## **Adding storage.**

<br>

  Amazon EC2 stores data on a network-attached virtual disk called Amazon Elastic Block Store (Amazon EBS).
  
  We launch the EC2 instance using a default 8 GiB disk volume. This is our root volume (also known as a boot volume).

13. In the **Configure storage** pane, keep the default storage configuration.

## **Configuring advanced details.**

<br>

14. Expand the **Advanced details** pane.

15. Select the dropdown for **Termination protection**, then choose **Enable**.

  When launching an instance in Amazon EC2, we have the option of passing user data to the instance. These commands can be used to perform common automated configuration tasks and even run scripts after the instance starts.

16. Copy the following commands, and paste them into the **User data** text box.

    ```bash
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello From our Web Server!</h1></html>' > /var/www/html/index.html
    ```

  The script does the following:
  
  - Install an Apache web server (httpd)
  
  - Configure the web server to automatically start on boot
  
  - Activate the Web server
  
  - Create a simple web page

## **Launching an EC2 instance.**

<br>

  Now that we have configured our EC2 instance settings, it is time to launch our instance.

17. In the right pane, choose **Launch instance**.

18. Choose **View all instances**.

  The instance appears in a **Pending** state, which means it is being launched. It then changes to **Running**, which indicates that the instance has started booting. There will be a short time before you can access the instance.
  
  The instance receives a public DNS name that you can use to contact the instance from the Internet.

19. Select the box next to our **Web Server**. The **Details** tab displays detailed information about our instance.

  To view more information in the **Details** tab, drag the window divider upward.
  
  Review the information displayed in the **Details, Security** and **Networking** tabs.

20. Wait for our instance to display the following:

  **Note**: Refresh if needed.
  
  - Instance State: Running
  
  - Status Checks: 2/2 checks passed

## **Monitor our Instance.**

<br>

  Monitoring is an important part of maintaining the reliability, availability, and performance of our Amazon Elastic Compute Cloud (Amazon EC2) instances and our AWS solutions.

21. Select the instance by checking the box next to the instance and navigate to the bottom of the screen to the **Status checks** tab.

  With instance status monitoring, you can quickly determine whether Amazon EC2 has detected any problems that might prevent our instances from running applications. Amazon EC2 performs automated checks on every running EC2 instance to identify hardware and software issues.
  
  Notice that both the **System reachability** and **Instance reachability** checks have passed.

22. Select the **Monitoring** tab.

  This tab displays Amazon CloudWatch metrics for our instance. Currently, there are not many metrics to display because the instance was recently launched.
  
  You can choose a graph to see an expanded view.
  
  Amazon EC2 sends metrics to Amazon CloudWatch for our EC2 instances. Basic (five-minute) monitoring is enabled by default. You can enable detailed (one-minute) monitoring.

23. In the **Actions** menu, select **Monitor and troubleshoot -> Get Instance Screenshot**.

  This shows you what our Amazon EC2 instance console would look like if a screen were attached to it.
  
  If you are unable to reach our instance via SSH or RDP, you can capture a screenshot of our instance and view it as an image. This provides visibility as to the status of the instance, and allows for quicker troubleshooting.

24. Select **Cancel** located at the bottom of the instance screenshot.

  **Congratulations!** You have explored several ways to monitor our instance.

## **Update our Security Group and Access the Web Server.**

<br>

  When you launched the EC2 instance, you provided a script that installed a web server and created a simple web page. Here, you will access content from the web server.

25. Select the instance by checking the box and select the **Details** tab.

26. Copy the **Public IPv4 address** of our instance to our clipboard.

27. Open a new tab in our web browser, paste the IP address you just copied, then press **Enter**.

  **Question**: Are we able to access our web server? Why not?
  
  We are **not** currently able to access our web server because the _security group_ is not permitting inbound traffic on port 80, which is used for HTTP web requests. This is a demonstration of using a security group as a firewall to restrict the network traffic that is allowed in and out of an instance.
  
  To correct this, we will now update the security group to permit web traffic on port 80.

28. Keep the browser tab open, but return to the **EC2 Management Console** tab.

29. In the left navigation pane, select **Security Groups** located under **Network & Security**.

30. Select **Web Server security group.**

31. Select the **Inbound rules** tab.

The security group currently has no rules.

32. Select **Edit inbound rules** then select **Add rule** and configure the rule with the following settings:

    - **Type**: `HTTP`

    - **Source**: `Anywhere-IPv4`

    Select **Save rules**

33. Return to the web server tab that you previously opened and refresh the page.

  You should see the message Hello From our Web Server!
  
  Congratulations! We have successfully modified our security group to permit HTTP traffic into our Amazon EC2 Instance.

## **Resize our Instance: Instance Type and EBS Volume**

<br>

  As our needs change, you might find that our instance is over-utilized (too small) or under-utilized (too large). 
  If so, we can change the instance type. For example, if a t3.micro instance is too small for its workload, we can change it to an m5.medium instance. 
  Similarly, we can change the size of a disk.

## **Stop our Instance**
<br>

  Before we can resize an instance, we must stop it.
  
  When we stop an instance, it is shut down. There is no charge for a stopped EC2 instance, but the storage charge for attached Amazon EBS volumes remains.

34. On the **EC2 Management Console**, in the left navigation pane, select **Instances**.

  **Web Server** should already be selected.

35. Select **Instance state > Stop instance**.

36. Select **Stop**

  Our instance will perform a normal shutdown and then will stop running.

37. Wait for the **Instance State** to display: stopped

## **Change The Instance Type**
<br>

38. In the **Actions** menu, select **Instance Settings Change Instance Type**, then configure:

  - **Instance Type**: *t3.small*
  
  - Select *Apply*

  When the instance is started again it will be a *t3.small*, which has twice as much memory as a t3.micro instance. 

## **Resize the EBS Volume.**
<br>

40. In the left navigation menu, select **Volumes** located under **Elastic Block Store**.

41. Select the volume by checking the box, and navigate to the **Actions** menu, select **Modify Volume**.

  The disk volume currently has a size of 8 GiB. we will now increase the size of this disk.

42. Change the size to: *10*.

43. Select **Modify**

44. Select **Modify** to confirm and increase the size of the volume.

## **Start the Resized Instance.**
<br>

  We will now start the instance again, which will now have more memory and more disk space.

45. In left navigation pane, select **Instances**.

46. Select the **Web Server** instance by checking the box, then navigate to **Instance state > Start instance**.

  Congratulations! We have successfully resized our Amazon EC2 Instance. Here we changed our instance type from *t3.micro* to *t3.small*. We also modified our root disk volume from 8 GiB to 10 GiB.

## **Test Termination Protection.**
<br>

  We can delete our instance when we no longer need it. This is referred to as *terminating* our instance. We cannot connect to or restart an instance after it has been terminated.
  
  Here, we will learn how to use *termination* protection.

47. In left navigation pane, select **Instances**.

48. Select the **Web Server** instance by checking the box and navigate to the top and select **Instance state** menu, select **Terminate instance**.

  **Note**: There is a message that says: *On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated. Storage on any local drives will be lost.* It will ask if we are sure that we want to terminate the instance. We will be able to select the **Terminate** button.
  
  **Note**: We will notice that the instance did not terminate and a red error message pops up at the top that says: *Failed to terminate an instance: The instance may not be terminated.* This is because it has termination protection enabled.

49. In the **Actions** menu, select **Instance settings -> Change termination protection**.

50. Uncheck **Enable** followed by **Save**

  We can now terminate the instance.

51. In the Actions menu, select **Instance State -> Terminate instance**.

52. Select **Terminate**

  **Congratulations!** We have successfully tested termination protection and terminated our instance.

