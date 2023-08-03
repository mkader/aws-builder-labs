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
1. In this lab, you use AWS Lambda as an event-driven compute service where AWS Lambda runs your code in response to changes to data in an SNS topic and an Amazon S3 bucket.
1. You can use AWS Lambda in two ways:
    1. As an event-driven compute service where AWS Lambda runs your code in response to events, such as uploading image files as you see in this lab.
    ![img](https://us-west-2-aws-training.s3.us-west-2.amazonaws.com/courses/spl-51/v3.2.13.prod-a2fdb582/images/lambda-howitworks.png)
    1. As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls.
    ![img](https://us-west-2-aws-training.s3.us-west-2.amazonaws.com/courses/spl-51/v3.2.13.prod-a2fdb582/images/lambda-mobilebackend.png)
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
![img](https://us-west-2-aws-training.s3.us-west-2.amazonaws.com/courses/spl-51/v3.2.13.prod-a2fdb582/images/lambda-etl.png)

### AMAZON KINESIS
1. Amazon Kinesis is a fully managed service for real-time processing of streaming data at massive scale. 
1. Amazon Kinesis can collect and process hundreds of terabytes of data per hour from hundreds of thousands of sources, allowing you to easily write applications that process information in real-time, from sources such as web site click-streams, marketing and financial information, manufacturing instrumentation and social media, and operational logs and metering data.
1. With Amazon Kinesis applications, you can easily send data to a variety of other services such as Amazon Simple Storage Service (Amazon S3), Amazon DynamoDB, Amazon Lambda, or Amazon Redshift. 
1. In a few clicks and a couple of lines of code, you can start building applications which respond to changes in your data stream in seconds, at any scale, while only paying for the resources you use.
![img](https://us-west-2-aws-training.s3.us-west-2.amazonaws.com/courses/spl-51/v3.2.13.prod-a2fdb582/images/lambda-iot.png)

## Part 1: Event-Driven Programming with Amazon Kinesis and AWS Lambda
### Task 1: Create an Amazon Kinesis Stream
1. **AWS Management Console** -> **Kinesis** -> Choose **Create data stream** service.
1. In **Data stream configuration** section, configure: **Data stream** name: Enter **Lab-Stream**
1. In **Data stream capacity** section, configure: **Capacity Mode**: Choose **Provisioned**, **Provisioned shards**: Enter **1**
1. Each shard supports a pre-defined capacity, as shown in the Total **data stream capacity** section.
1. This lab only requires one shard, but applications requiring more capacity can simply request more shards.
1. Choose **Create data stream**.
1. A message is displayed on the page with the following message: **Data stream Lab-Stream successfully created..**
1. **Congratulations!** You have successfully created the Amazon Kinesis data stream.

### Task 2: Create a Lambda Function
1. **AWS Management Console** -> **Lambda** -> Choose **Create a function**.
1. You start by selecting a Lambda blueprint. Blueprints are pre-built for you and can be customized to suit your specific needs

Choose Use a blueprint .

In the Basic information section, configure:

Click on the Select blueprint search  box

Search for: 

kinesis-process-record-python
.

Choose Process records sent to a Kinesis stream .

Enter below details:

Function name: Enter 

ProcessKinesisRecords
Execution role: Choose  Use an existing role
Existing role: Select lambda_basic_execution from the drop-down list.
In the Kinesis trigger section, configure:
Kinesis stream: Enter 

Lab-Stream
This will configure the Lambda function so that it is triggered whenever data comes into the Kinesis stream you created earlier.

Leave all other settings on the page at the default values.

Scroll down and examine the Lambda blueprint displayed in the Lambda function code section. It does the following:

Loop through each of the records received
Decode the data, which is encoded in Base 64
Print the data to the debug log
At the bottom of the screen, choose Create function.
This function is triggered whenever data is sent to the stream.

 Congratulations! You have successfully created the AWS Lambda function.

### Task 3: Test your Function
In this task, you simulate data coming from a stream to trigger your Lambda function.

Choose the Test tab.
 Note: An event template for Kinesis will be automatically selected. The event contains a simulated message arriving via Kinesis.

The Test event page is displayed.

For Event Name: Enter 

stream

Choose Save

Choose Test

The output should look similar to this:

 Execution result: succeeded

A new tab with name Execution result will automatically open with output of the Lambda function.
You can see the following information about the Lambda execution in the Summary section:

Duration
Billed Duration
Maximum memory used
Function Logs
Choose the Test button, another three times, waiting a few seconds between each test.
This generates the test data for Amazon CloudWatch.

Choose the Monitor tab.
You can now view the CloudWatch metrics for your Lambda function. Metrics should be available for Invocations and Duration.

 If the metrics do not appear, wait a minute and then choose  refresh.

 Congratulations! You have successfully tested the AWS Lambda function.

Part 2: Event Driven Programing with Amazon DynamoDB and AWS Lambda
In the second half of this lab, you learn about a different event driven programming method, this time with DynamoDB and Lambda.

Task 4: Create Tables in DynamoDB
In this task, you create a DynamoDB table that contains scores for online games.

In the AWS Management Console, use the AWS search bar to search for 

DynamoDB
 and then choose the service from the list of results.

Choose Create table and configure:

Table name: Enter 

GameScoreRecords
Partition key: Enter 

RecordID
 and select Number from the drop-down list
Choose Create table.
You now create another table for linking scores to users.

Choose Create table and configure:
Table name: Enter

GameScoresByUser
Partition key: Enter 

Username
 and select String from the drop-down list
Choose Create table.
You now activate DynamoDB Streams on the first table. This generates streaming data whenever there is any change to the table (insert, update, delete).

Choose the first table you created, GameScoreRecords.

On the Exports and Streams tab, Scroll to DynamoDB stream details, choose Enable.

In the Enable DynamoDB stream page, select:

View type:  New image
Choose Enable stream.
Any record sent to this table now sends a message via DynamoDB streams, which triggers the Lambda function.

 Congratulations! You have successfully created the DynamoDB tables.

Task 5: Create a Lambda Function
In this task, you create a Lambda function that will be triggered by updates to your DynamoDB table.

In the AWS Management Console, use the AWS search bar to search for 

Lambda
 and then choose the service from the list of results.

Choose Create function.

You will be providing the code to run, so choose Author from scratch .

Configure the following:

Function name: Enter 

AggregateScoresByUser
Runtime: Choose Node.js 16.x
Expand  Change default execution role
Execution role:  Use an existing role
Existing role: Select lambda_basic_execution_dynamodb from the drop-down list
Choose Create function.

Select the Code tab, then:

Delete all of the code in the index.js editor

Copy and paste this code into the index.js editor:


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
Examine the code. It does the following:

Loop through each incoming record
Create (ADD) an item in the GameScoresByUser table with the incoming score
Wait until all updates have been processed
Choose Deploy.
You now configure the function to execute when a value is added to the DynamoDB table.

Scroll up to the Function overview section.

Chooser  Add trigger then configure:

In the Trigger configuration section, configure the following:

Select a source: Choose 

DynamoDB
 from the drop-down list.
DynamoDB table: Choose 

GameScoreRecords
Choose Add.
 Note: The function is triggered when a new game score is added to the DynamoDB table. You can now test the function with a record that simulates an update of the database.

Choose Test tab.

For Event name: Enter 

score

Delete the existing test code (with key3, etc).

Copy and paste this record into the test event window:


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
Examine the test record. It is simulating an incoming record from the GameScoreRecords table.

Choose Save.

Choose Test.

 Verify: Confirm that the Lambda function has been invoked. A banner at the top of the screen should state:

 Execution result: succeeded

Expand  Details to view the output of the Lambda function.
You should see: Successfully processed 1 records.

VERIFY IN DYNAMODB
You now verify that the data was updated in DynamoDB.

In the AWS Management Console, use the AWS search bar to search for 

DynamoDB
 and then choose the service from the list of results.

In the navigation panel on the left side of the screen, choose Tables.

Select  GameScoreByUser.

On the top right side of the page, open the Actions  dropdown menu, then choose Explore Items

 Note: This table was previously empty (you created it yourself), but you should now see an entry for Jane Doe.

TRIGGER THE UPDATE
You can perform more tests by inserting values in the Scores table, and confirming that the Lambda updates the User table.

Select the  GameScoreRecords table.

Under Items returned section, choose Create item.

 Note: In the following steps, you create a new item and assign it two attributes.

Enter 

1
 in the text field next to RecordID.

Open the Add new attribute  dropdown menu and follow the steps below to add a username attribute:

Choose String from the drop-down list
Attribute Name: Enter 

Username
Value: Enter a person’s name
Open the Add new attribute  dropdown menu and follow the steps below to add a score attribute:
Choose Number from the drop-down list
Attribute Name: Enter 

Score
Value: Enter a random score
Choose Create item.
 Verify: Confirm that your new item is displayed. It should have also triggered the Lambda function, resulting in a new entry in the other table.

Select the  GameScoresByUser table.
You should see that the new data you entered has been copied to the User table.

 Note: Refresh  the page, if you do not see the data populated automatically.

Repeat the test by adding more items in the GameScoreRecords table.

 Congratulations! You have successfully triggered the Lambda function.

Conclusion
 Congratulations! You now have successfully:

Created a Lambda function from a blueprint.
Created an Amazon Kinesis Stream and used it to trigger your Lambda function.
Used CloudWatch to monitor your function.
Create an Amazon DynamoDB table and inserted sample data.
Enabled Amazon DynamoDB Streams.
Tested and enabled the Lambda function on an Amazon DynamoDB table.
