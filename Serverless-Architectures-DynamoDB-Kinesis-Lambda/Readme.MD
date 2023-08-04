https://learn.labs.awsevents.com/sa/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2Fspl-51-PromoLab%3Av2/en-US

# Serverless Architectures with Amazon DynamoDB and Amazon Kinesis Streams with AWS Lambda

## Lab Overview
1. **Part 1** - create a Lambda function from a blueprint, create an Amazon Kinesis Stream, then trigger the function with data from your stream and monitor the process with Amazon CloudWatch.
2. **Part 2** - Learn the basics of event-driven programming using Amazon DynamoDB, DynamoDB Streams, and AWS Lambda. You learn the process of building a real-world application using triggers that combine DynamoDB Streams and Lambda.

## OBJECTIVES
1. By the end of this lab, you will be able to do the following:
    1. Create an AWS Lambda function from a blueprint
    1. Create an Amazon Kinesis Stream
    1. Use Amazon CloudWatch to monitor Kinesis event data triggering your Lambda function
    1. Create an Amazon DynamoDB table and insert items
    1. Enable the Amazon DynamoDB Streams feature
    1. Configure and troubleshoot Lambda functions

## About the Technologies
### AWS LAMBDA
1. Lambda is a compute service that provides resizable compute capacity in the cloud to make web-scale computing easier for developers. 
1. You can upload your code to AWS Lambda and the service can run the code on your behalf using AWS infrastructure. 
1. AWS Lambda supports multiple coding languages, including Node.js, Java, or Python. 
1. After you upload your code and create a Lambda function, AWS Lambda takes care of provisioning and managing the servers used to run the code.
1. You can use AWS Lambda in two ways:
    1. As an event-driven compute service where AWS Lambda runs your code in response to events, such as uploading image files as you see in this lab.
    ![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/Serverless-Architectures-DynamoDB-Kinesis-Lambda/lambda-howitworks.png)
    1. As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls.
    ![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/Serverless-Architectures-DynamoDB-Kinesis-Lambda/lambda-mobilebackend.png)
1. Lambda passes on to you the financial benefits of Amazonâ€™s scale. 
1. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. 
1. With these capabilities, you can use Lambda to easily build data processing triggers for AWS services like Amazon S3 and Amazon DynamoDB, process streaming data stored in Amazon Kinesis, or create your own back end that operates at AWS scale, performance, and security.

### LAMBDA BLUEPRINTS
1. Blueprints are sample configurations of event sources and Lambda functions that do minimal processing for you. 
1. Most blueprints process events from specific event sources, such as Amazon S3 or DynamoDB. 
1. For example, if you select an s3-get-object blueprint, it provides sample code that processes an object-created event published by Amazon S3 that Lambda receives as parameter.
1. When you create a new AWS Lambda function, you can use a blueprint that best aligns with your scenario. You can then customize the blueprint as needed. You do not have to use a blueprint (you can author a Lambda function and configure an event source separately).

### AMAZON DYNAMODB
1. Amazon DynamoDB is a NoSQL db service for all applications that need consistent, single-digit millisecond latency at any scale. 
1. It is a fully managed db and supports both document and key-value data models. 
1. Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT, and many other applications.
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/Serverless-Architectures-DynamoDB-Kinesis-Lambda/lambda-etl.png)

### AMAZON KINESIS
1. Amazon Kinesis is a fully managed service for real-time processing of streaming data at massive scale. 
1. Amazon Kinesis can collect and process hundreds of terabytes of data per hour from hundreds of thousands of sources, allowing you to easily write applications that process information in real-time, from sources such as web site click-streams, marketing and financial information, manufacturing instrumentation and social media, and operational logs and metering data.
1. With Amazon Kinesis applications, you can easily send data to a variety of other services such as Amazon Simple Storage Service (Amazon S3), Amazon DynamoDB, Amazon Lambda, or Amazon Redshift. 
1. In a few clicks and a couple of lines of code, you can start building applications which respond to changes in your data stream in seconds, at any scale, while only paying for the resources you use.
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/Serverless-Architectures-DynamoDB-Kinesis-Lambda/lambda-iot.png)

## Part 1: Event-Driven Programming with Amazon Kinesis and AWS Lambda
### Task 1: Create an Amazon Kinesis Stream
1. **AWS Console** -> **Kinesis** -> **Create data stream** service.
1. In **Data stream configuration** section, configure: **Data stream** name: Enter **Lab-Stream**
1. In **Data stream capacity** section, configure: **Capacity Mode**: **Provisioned**, **Provisioned shards**: Enter **1**
1. Each shard supports a pre-defined capacity, as shown in the Total **data stream capacity** section.
1. This lab only requires one shard, but applications requiring more capacity can simply request more shards.
1. Choose **Create data stream** -> Msg displayed **Data stream Lab-Stream successfully created.**

### Task 2: Create a Lambda Function
1. **AWS Console** -> **Lambda** -> **Create a function** ->  **Use a blueprint**
    1. Using a Lambda blueprint. Blueprints are pre-built for you and can be customized to suit your specific needs
1. **Basic information** section -> **Select blueprint** search box -> enter **kinesis-process-record-python**.
1. **Function name**: **ProcessKinesisRecords**, **Execution role**: **Use an existing role**, **Existing role**: Select **lambda_basic_execution**.
1. **Kinesis trigger** section -> **Kinesis stream**: select **Lab-Stream**
    1. This will configure the Lambda function so that it is triggered whenever data comes into the Kinesis stream you created earlier.
1. Leave all other settings on the page at the default values.
1. Examine the Lambda blueprint displayed in the **Lambda function code** section. It does the following:
    1. Loop through each of the records received
    1. Decode the data, which is encoded in Base 64
    1. Print the data to the debug log
1. At the bottom of the screen, choose **Create function**, it triggered whenever data is sent to the stream.

### Task 3: Test your Function
1. In this task, you simulate data coming from a stream to trigger your Lambda function.
1. Choose the **Test** tab -> **Event Name**: **stream** -> **Save** -> **Test**
    1. **Note:** An event template for Kinesis will be automatically selected. The event contains a simulated message arriving via Kinesis.
1. The output -> **Execution result: succeeded**. Expand **Details** -> to see about the Lambda execution in the Summary information in th section: **Duration**, **Billed Duration**, **Maximum memory used**, **Function Logs**.
1. **Test** -> Click **three times**, waiting a few seconds between each test. This generates the test data for **Amazon CloudWatch**.
1. **Monitor** tab -> View the **CloudWatch metrics for your Lambda function** -> **Invocations and Duration**.
    1. If the metrics do not appear, wait a minute and then choose  **refresh**.

## Part 2: Event Driven Programing with Amazon DynamoDB and AWS Lambda
### Task 4: Create Tables in DynamoDB - contains scores for online games.
1. **AWS Console** -> **DynamoDB** -> **Create table** 
    1. **Table name**: **GameScoreRecords**, **Partition key**: **RecordID**
 -> **Number** -> **Create table**.
    1. **Create table** -> **Table name**: **GameScoresByUser**, **Partition key**: **Username** -> **String** -> **Create table.**
1. You now activate DynamoDB Streams on the first table. This generates streaming data whenever there is any change to the table (insert, update, delete).
    1. Choose **GameScoreRecords** -> **Exports and Streams** tab -> **DynamoDB stream** details -> **Turn on**.
    1. **Enable DynamoDB stream** page, select: **View type:**  **New image** -> **Enable stream**.
1. Any record sent to this table now sends a message via DynamoDB streams, which triggers the Lambda function.

## Task 5: Create a Lambda Function - it will be triggered by updates to your DynamoDB table.
1. **AWS Console** -> **Lambda** -> **Create function** -> Providing the code to run, **Author from scratch**.
1. Configure the following: **Function name:** **AggregateScoresByUser**, **Runtime:" ** Node.js 18.x**
1. Expand  **Change default execution role** -> **Execution role**:  **Use an existing role**, **Existing role**: Select **lambda_basic_execution_dynamodb**
1. Choose **Create function** -> Select the **Code** tab, then: **Delete** all code, **Copy and paste** this code into the **index.js** editor:
    ```
        // Set up AWS client
        var AWS = require("aws-sdk");
        var dynamodb = new AWS.DynamoDB();
        
        exports.handler = function (event, context) {
          // Keep track of how many requests are in flight
          var inflightRequests = 0;
        
          event.Records.forEach(function (record) {
            console.log("DynamoDB Record: %j", record.dynamodb);
            // Get the new image of the DynamoDB Streams record
            var newItemImage = record.dynamodb.NewImage;
        
            // Set the appropriate parameters for UpdateItem
            // Refer to the ADD operation in the UpdateItem API for UpdateExpression
            // http://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_UpdateItem.html
            // Adds the specified value to the item, if attribute does not exist, set the attribute
            var updateItemParams = {
              TableName: "GameScoresByUser",
              Key: { Username: newItemImage.Username },
              UpdateExpression: "ADD Score :attrValue",
              ExpressionAttributeValues: { ":attrValue": newItemImage.Score },
            };
        
            // Make a callback function to execute once UpdateItem request completes
            // It may be helpful to refer to the updateItem method for the Javascript SDK
            // http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html\#updateItem-property
            var updateItemCallback = function (err, data) {
              if (err) {
                // log errors
                console.log(err, err.stack);
              } else {
                // check if all requests are finished, if so, end the function
                inflightRequests--;
                if (inflightRequests === 0) {
                  context.succeed(
                    "Successfully processed " + event.Records.length + " records."
                  );
                }
              }
            };
        
            // Send UpdateItem request to DynamoDB
            dynamodb.updateItem(updateItemParams, updateItemCallback);
            // Increase count for number of requests in flight
            inflightRequests++;
          });
        
          // If there are no more requests pending, end the function
          if (inflightRequests === 0) {
            context.succeed(
              "Successfully processed " + event.Records.length + " records."
            );
          }
        };
    ```
    1. Examine the code. It does the following:
        1. Loop through each incoming record
        1. Create (ADD) an item in the GameScoresByUser table with the incoming score
        1. Wait until all updates have been processed
1. Choose **Deploy**.
1. You now configure the function to execute when a value is added to the DynamoDB table.
1. **Function overview** section -> Chooser **Add trigger** -> **Select a source**: **DynamoDB**, **DynamoDB table**: **GameScoreRecords** -> Choose **Add**.
    1. **Note**: The function is triggered when a new game score is added to the DynamoDB table. You can now test the function with a record that simulates an update of the database.
1. Choose **Test** tab -> **Event name**: **score**
1. **Delete** the existing test code (with key3, etc). **Copy and paste** this record into the test event window:
    ```
        {
          "Records": [
            {
              "eventID": "1",
              "eventVersion": "1.0",
              "dynamodb": {
                "Keys": { "RecordID": { "N": "2" } },
                "NewImage": {
                  "RecordID": { "N": "2" },
                  "Username": { "S": "Jane Doe" },
                  "Score": { "N": "100" },
                  "Nickname": { "S": "JaneD" }
                },
                "StreamViewType": "NEW_IMAGE",
                "SequenceNumber": "111",
                "SizeBytes": 26
              },
              "awsRegion": "us-west-2",
              "eventName": "INSERT",
              "eventSourceARN": "arn:aws:dynamodb:us-west-2:account-id:table/GameScoreRecords/stream/2015-10-07T00:48:05.899",
              "eventSource": "aws:dynamodb"
            }
          ]
        }
    ```
    1. **Examine** the test record. It is simulating an incoming record from the GameScoreRecords table.
1. **Save** -> **Test** -> **Verify:** **Execution result: succeeded**
    1. Expand **Details** to view the output of the Lambda function. You should see: **Successfully processed 1 records.**

### VERIFY IN DYNAMODB - that the data was updated in DynamoDB.
1. **AWS Console** -> **DynamoDB** -> **Tables** -> **GameScoreByUser** -> **Actions**  dropdown menu -> choose **Explore Items**
    1. **Note:** This table was previously empty (you created it yourself), but you should now see an entry for Jane Doe.

### TRIGGER THE UPDATE
1. You can perform more tests by inserting values in the Scores table, and confirming that the Lambda updates the User table.
1. Select the **GameScoreRecords** table -> Under **Items returned** section ->choose **Create item**.
    1. **Note**: In the following steps, you create a new item and assign it two attributes.
1. Enter **1** in the text field next to **RecordID**.
1. **Add new attribute**, **String** - > **Attribute Name**: **Username**, **Value**: **Enter a person’s name**
1. **Add new attribute**, "Number" -> **Attribute Name: Score**, "Value: Enter a random score**
1. Choose **Create item**.
1. **Verify**: Confirm that your new item is displayed. It should have also triggered the Lambda function, resulting in a new entry in the other table.
1. Select the **GameScoresByUser** table.
1. You should see that the new data you entered has been copied to the User table.
1. **Note:** Refresh  the page, if you do not see the data populated automatically.
1. **Repeat the test** by adding more items in the **GameScoreRecords** table.
