# Evolution of a Website: Going from Single Server to Serverless

https://learn.labs.awsevents.com/sa/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FSPL-TF-100-NWSSTS-1-PromoLab%3Av2/en-US

## Lab Overview
1. Create and maintain the ice cream company website. In this lab, you follow the growth of the business and how it evolves their website’s infrastructure.
1. Creating the company’s static website hosted on Amazon Simple Storage Service (S3) that displays the ice cream flavors. 
1. The business starts to gain popularity among the locals and flavors start to sell out before the day is up. 
1. The business owners ask you to migrate to a server where they can have you push updates. 
1. They decide to migrate their static site to Amazon Elastic Compute Cloud (EC2) with a Linux, Apache, MySQL, PHP (LAMP) stack built on it.
1. This will allow the website to update and react dynamically. 
1. Demand for Company’s ice cream surges after a viral video and the infrastructure and maintenance becomes difficult to manage. 
1. To scale with demand, you suggest migrating sections of their LAMP stack to serverless services, starting with the MySQL database. 
1. The MySQL database will be replaced with Amazon DynamoDB serverless database.

## LAB OBJECTIVES
1. Upload files to Amazon S3 to host a static website.
1. Migrate the dynamic website hosted on EC2 from using a MySQL database to using Amazon DynamoDB.
1. Update the DynamoDB table with a new ice cream flavor.
1. Refresh the website to reveal your new ice cream flavor.

## APPLICATION ARCHITECTURE
### The application architecture uses the following services:
1. **Static site:** Amazon S3 which contains HTML, CSS, and images.
1. **Dynamic site:** Amazon EC2 Linux, Apache web server, MySQL database, and PHP (LAMP) stack will come pre installed. This will allow the owner or developer to update the flavors available for the day.
1. **Serverless:** Amazon DynamoDB. This will replace the MySQL database and make the database portion serverless.
    
![img](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/architecture_overview.png)
    
The architecture has a client accessing Amazon S3, an Amazon EC2 instance with a LAMP stack installed on it, and the same Amazon EC2 instance with the database replaced with Amazon DynamoDB.

## Task 1: Upload files to Amazon S3
1. Create a static website. To help speed up this step, this task is already configured with an S3 bucket and the following actions have already been completed.
    1. The [website hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EnableWebsiteHosting.html) feature is enabled to allow this bucket to host a website.
    1. Additionally, a **bucket policy** was added to allow **s3:GetObject** permissions to this bucket and the files within the bucket.
    1. The **index document** property has been set to **index.html** in the root of the bucket. This is a file you just copied to the bucket.
1. Download and unzip the **[static-site.zip](https://us-west-2-aws-training.s3.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/scripts/static-site.zip)** file containing the static website files to your local machine.
1. **AWS Management Console** -> **S3** -> Choose the bucket with **s3htmlbucket**.
1. From local machine, drag all files and folders to the **Files and folders** area of the **Object** page.
1. Choose **Upload** at the bottom of the page.  Expected service output: **Upload succeeded**
1. Choose the **Close** button. Your bucket listing should look similar to the example below:
![](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/bucket-layout1.png)
1. Choose the **Properties** tab -> **Static website hosting** section -> browse the S3 website URL.

### AWS Images
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.0.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.1.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.2.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.3.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.4.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.5.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab1.6.PNG)

## Task 2: Verify the dynamic website on Amazon EC2 is running
1. The business starts to grow, and customers want a feature on the website where they can see an updated flavor of the day, or when a flavor sells out. 
1. You suggest to migrate the server to an Amazon EC2 instance with LAMP installed on it. 
1. This allows the site to generate dynamic content using a database and allows you to update flavors as inventory is added or runs out.
1. To help speed up this step, this task is already configured with a Amazon VPC and its networking resources, and an Amazon EC2 with a LAMP stack configured. The following actions have already been completed.
    1. The [LAMP stack](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html) is installed as a web server to host AnyCompany’s dynamic website.
    1. Additionally, an IAM Policy is used to define the permissions for each service used.
    1. Flavors and prices are now updated through a MySQL database that dynamically updates the website.
1. **Architecture Overview**
    ![](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/dynamic_site.png)
    1. In this architecture, the OS for this Amazon EC2 instance is Linux, the HTTP server is Apache, the MySQL database is MariaDB, and the scripting language used is PHP.
    1. All of these configured together, run the dynamic website you open shortly. 
    1. A dynamic site is information or web pages that are generated in real time. With a LAMP stack, customers will be able to input orders, and submit them.

1. **AWS Management Console** -> **EC2** -> **Instances** -> check **Ice Cream** name.

1. **Details** tab -> browse the **Public IPv4 IP** address (use http) value. Leave this tab **open** for **task 3**.
1. Expected website: 
![img](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/dynamic_website.png)

### AWS Images
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab2.0.PNG)

## Task 3: Update dynamic website to a serverless solution using DynamoDB for the MySQL database
1. Business has been steadily growing but with a recent video of a customer’s experience that has since gone viral, the AnyCompany Ice Cream website has experienced more web traffic than the current MySQL database server can keep up with. 
1. Unfortunately, customers are getting errors accessing the website. 
1. To remedy this, you are tasked with migrating the local MySQL database to Amazon DynamoDB since it can scale with the demands of customer web traffic to ensure customers continue to reach the AnyCompany Ice Cream website without interruptions.
1. **Note:** The Amazon DynamoDB database has already been created and the existing MySQL table has already been imported.
    1. The [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) table is installed, however, the items in the table need to be created.
    1. A script is created to replace the old index.php with a new index.php.
    1. IAM user, and roles are adjusted to allow permissions to access Amazon DynamoDB.
1. **Architecture Overview**
![img](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/serverless.png)
    1. In this architecture, the OS for this Amazon EC2 instance is Linux, the HTTP server is Apache, the MySQL database has been replaced with an Amazon DynamoDB serverless database, and the scripting language used is PHP. 
    1. All of these configured together run the dynamic website you open shortly. A dynamic site is information or web pages that are generated in real time.

### TASK 3.1: UPDATE THE AMAZON DYNAMODB PRODUCTS TABLE WITH A NEW ICE CREAM FLAVOR
1. **AWS Console** -> **DynamoDB** -> **Tables** -> **Products** (table name) -> **Explore table items**.
1. In the **Items returned** section, choose **Create item** and configure the following options:
    1. **Attribute name:** id - Partition key, **Value:** 1
1. Choose Add **new attribute**  > **String** and then configure the following options:
    1. **Attribute name:** ProductFlavor, **Value:** Cinnamon-spiced Chocolate
1. Choose Add **new attribute**  > **String** and then configure the following options:
    1. **Attribute name:** ProductPrice, **Value:** 7.99
1. Choose **Create Item**.

### TASK 3.2: UPDATE THE WEBSITE TO USE AMAZON DYNAMODB
1. **AWS Console** -> **EC2** -> **Ice Cream** instance name -> choose the **Connect** .
1. From the **Connect to instance** page, choose the **EC2 Instance Connect** tab and verify that the value for **User name** is set to **ec2-user**, then choose **Connect** .
1. **Command:** From the terminal session copy and paste the following command to migrate the website from using the MySQL database to Amazon DynamoDB.
    1. /var/www/html/migrate-database.sh
    2. **Expected output:**
        ```
        ******************************
        **** This is OUTPUT ONLY. ****
        ******************************
        
        Renaming index.php to compute-index.php
        Renaming serverless-index.php to index.php
        Restarting Apache
        ```

### TASK 3.3 REFRESH WEBSITE TO SEE YOUR NEW ICE CREAM FLAVOR
1. Backto the browser, **refresh** to see your new ice cream flavor.
1. **Expected website**:
![img](https://us-west-2-tcprod.s3.us-west-2.amazonaws.com/courses/SPL-TF-100-NWSSTS/v1.0.2.prod-0837bec4/instructions/en_us/images/website-dynamodb-product-listing.png)
1. The website is updated with the new flavor and price listed from the Amazon DynamoDB Products table that was just created.

### AWS Images
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab3.0.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab3.1.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab3.2.PNG)
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/SingleServer_Serverless/lab3.3.PNG)
