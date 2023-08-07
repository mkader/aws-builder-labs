https://learn.labs.awsevents.com/sa/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FSPL-TF-300-MBFIFE-1-PromoLab%3Av2/en-US

### Frontend

## Lab overview
1. Build a serverless, full-stack single-page app with authentication that allows users to launch and terminate EC2 instances.
1. Use AWS Amplify to add AWS services and integrate them with your frontend. This includes adding authentication via Amazon Cognito and creating a secure REST API powered by Amazon API Gateway. You further extend the backend with a serverless function via AWS Lambda that interacts with the Amazon EC2 service.
1. Use AWS Amplify to add hosting your app, allowing it to be delivered to end-users on the AWS content delivery network using Amazon CloudFront. All application development happens within the AWS Cloud9 IDE.

## OBJECTIVES
1. Create and host a full-stack web app on AWS.
1. Implement React concepts such as states, effects and props.
1. Create auth, API, function, and hosting resources on AWS using AWS Amplify.
1.Integrate your frontend with backend services.

## AWS Amplify
1. It is a set of purpose-built tools and features that lets frontend web and mobile developers quickly and easily build full-stack applications on AWS. 
1. With Amplify, developers are able to deliver rich cloud-enabled experiences by integrating a broad range of capabilities into their apps including authentication, cloud storage, databases, REST & GraphQL APIs, online/offline data, serverless business logic, location, push notifications and real-time interactive experiences.
1. Amplify applications are powered by AWS managed services like AWS Lambda, AWS AppSync, Amazon API Gateway, Amazon Cognito, Amazon DynamoDB and Amazon Location Service. 
1. Amplify Hosting is a fully-managed web hosting service that serves content with low latency from Amazon’s reliable content delivery network (CDN) with hundreds of points of presence globally.

## Task 1: Create and run a blank React app in AWS Cloud9
1. React is a declarative, component-based JavaScript library used to build user interfaces. ***Create React App (CRA)*** is a CL tool used to quickly create single-page React applications.
1. Use CRA to generate the React development environment for your EC2 Vendor app. You also start a local debugging session to preview your running app.
1. **Note:** You use **React functional components** throughout this lab. Conceptually, components are like JavaScript functions. They accept arbitrary inputs and return React elements describing what should appear on the screen.

### TASK 1.1: LAUNCH THE AWS CLOUD9 IDE
1. From the **Lab** -> copy the **Cloud9Environment** URL in a new browser
1. This section of the IDE is where you update various file throughout this lab.

### TASK 1.2: GENERATE THE REACT DEVELOPMENT ENVIRONMENT
1. AWS Cloud9 terminal -> run the commands to invoke CRA to generate the React development environment for your app:
    ```
    cd ~/environment
    npx create-react-app ec2-vendor
    ```
1. For **Ok to proceed? (y)**,  install the package and CRA creates the environment.
1. Expected output: Output has been truncated.
    ```
    ******************************
    **** This is OUTPUT ONLY. ****
    ******************************
    
    Creating a new React app in /home/ec2-user/environment/ec2-vendor.
    
    Success! Created ec2-vendor at /home/ec2-user/environment/ec2-vendor
    Inside that directory, you can run several commands:
    
      npm start
        Starts the development server.
    
      npm run build
        Bundles the app into static files for production.
    
      npm test
        Starts the test runner.
    
      npm run eject
        Removes this tool and copies build dependencies, configuration files
        and scripts into the app directory. If you do this, you canâ€™t go back!
    
    We suggest that you begin by typing:
    
      cd ec2-vendor
      npm start
    
    Happy hacking!
    ```
1. Explore the **ec2-vendor** directory structure of the app.
    1. **node_modules** contains dependencies used by the app.
    1. **public** contains your static files. index.html is the page template and must exist in this directory.
    1. **src** contains dynamic components.
        1. **index.js** is the JS entry point and must exist in this directory. It provides **Document Object Model (DOM)** specific methods that can be used at the top level of your app and as an escape hatch to get outside the React model if you need to.
        1. By default, a React root is created for an element with the root ID, i.e. all the compiled React content goes into an HTML element with **id of root**. Hint: look for this element in index.html.
        1. Within **index.js**, the script imports a **component** named **App** and React renders this component inside its root.
    1. **package.json** contains the overall configuration for the React project.
        1. **Note:** There is one **@babel dependency** required that was **not installed by default**. Update the package.json file with this dependency.
    1. **Update the package.json** file to include the missing **@babel dependency** with the following command:
    ```
    jq '.dependencies = {"@babel/plugin-proposal-private-property-in-object": "^7.14.5"} + .dependencies' /home/ec2-user/environment/ec2-vendor/package.json > /home/ec2-user/environment/ec2-vendor/package.json.tmp && mv /home/ec2-user/environment/ec2-vendor/package.json.tmp /home/ec2-user/environment/ec2-vendor/package.json
    ```
1. To **start the app locally** on your AWS Cloud9 instance:
    ```
    cd ~/environment/ec2-vendor
    npm run start
    ```
1. **AWS Cloud9 toolbar** -> **Preview** -> **Preview Running Application**.

## Task 2: Update the React app to use traditional HTML elements for the EC2 Vendor app
1. In this task, you remove the CRA-created content and use standard HTMl elements to build an outline of your app.
1. **2nd AWS Cloud9 terminal** -> run the following commands to remove the existing components:
    ```
    cd ~/environment/ec2-vendor/src
    rm -f *
    ```
1. **Note:** See **1st terminal window**, notice how the compiler returns errors. This is because the local running app compiles itself whenever the app changes and you have just removed the JavaScript entry point.
1. From **2nd terminal window** and run the command to copy the HTML outline code into the app:
    ```
    cp ~/environment/lab-resources/frontend/example1/* ~/environment/ec2-vendor/src
    ```
1. **Refresh** the preview.  Explore the output code in ec2-vendor/src/App.js:
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/React-Amplify-Cloud9-AMI-Lambda/task2.png)

## Task 3: Update the app to use Amplify UI components
1. Use **Amplify UI**, a component library, instead of HTML elements to build your app. 1. A **component library** is a repository of customizable and reusable components that allow developers and designers to work consistently and save time. 
1. For example, instead of using &lt;button /> with CSS styling, you can import &lt;Button /> from a component library that provides additional features and styling built in.
1. Run the commands to install the Amplify UI component library:
    ```
    cd ~/environment/ec2-vendor
    npm install @aws-amplify/ui-react
    ```
1. Run the following command to update your code to use the component library:
    ```
    cp ~/environment/lab-resources/frontend/example2/* ~/environment/ec2-vendor/src
    ```
1.  Refresh the preview. It will take time to display, so wait for it
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/React-Amplify-Cloud9-AMI-Lambda/task3.png)
    1. Explore the code in **&lt;App />**:
        1. You import several components from **@aws-amplify/ui-react**. Component libraries provide documentation on their components’ use.
        1. Instead of &lt;div />, you wrap everything in a &lt;Card />. 
        1. Instead of &lt;header /> you use the component library’s &lt;Heading />. 
        1. While some HTML elements have a 1:1 mapping to a component library component, some do not. 

## Task 4: Update the app to include states
1. A **React state** is a built-in object that is used to store data belonging to that component. 
1. It should be used if the component needs to modify this data at some point. 
1. It should not be used if this value is derived from another value. 
1. When a component’s state changes, the component and any child components rerender.
1. **for example**, many websites provide an option to **switch between a light or dark theme**. When you first load the website, a theme state is created and contains an initial value. When you switch themes, you update the state value, and the website’s components are rerendered with the new theme options.
1. Update your app to support a dark or light theme, allow loading states in the buttons, and implement conditional rendering.
1. Run the command to update your code to use the states:
    ```
    cp ~/environment/lab-resources/frontend/example3/* ~/environment/ec2-vendor/src
    ```
1. Refresh the preview.
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/React-Amplify-Cloud9-AMI-Lambda/task4.png)
    1. Explore the code in <App />:
    1. To be able to use states, you **import useState** from react at the top of the file.
    1. The states are defined immediately after the function definition. The syntax is **const [value, setValue] = useState(defaultValue)**. Note that setvalue is a function that allows updates to value.
    1. The **isLaunching and isSigningOut states** are set to false initially. Notice on their respective buttons’ onClick actions, you use **setIsLaunching and setIsSigningOut** to set the states to true. The Amplify UI component library’s &lt;Button /> component has a **isLoading** property, which when set to true, disables the button and displays the text in the loadingText property with a spinning circle.
    1. The **reset button** is only displayed when **isLaunching or isSigningOut is set to true**. This is conditional rendering. When this button is chosen, both states are set back to false. This causes those two buttons to revert to their original state, and the reset button is removed from view.
    1. The **colorMode state** uses the Amplify UI component library’s built-in theming to render the page based on its value. Notice the **&lt;Card />** component is now wrapped in a **&ll;ThemeProvider />** component. This is a feature of Amplify UI that allows you to dynamically change the theme of your app.

## Task 5: Update the app to include effects, props and a reusable component
1. **React effect hooks**, implemented by **useEffect**, adds the ability to perform side effects from a function component. Examples of this include fetching data and timers.
1. **React props** are arguments passed into React components.
1. **Reusable components is one of the core concepts of atomic design**. As atoms are the building blocks matter, in UI development, you can think of these atoms as basic UI components. For example, in your app’s table, the &lt;TableBody /> section contains the same few rows of &lt;TableCell /> surrounded by a &lt;TableRow />. What if you created a reusable component that returned the table row given the row’s parameters?
1. Update your app with the ability to dynamically add and remove instances.
1. Run the command to update your code to use the effects and reusable components:
    ```
    cp ~/environment/lab-resources/frontend/example4/* ~/environment/ec2-vendor/src
    ```
1. Refresh the preview.
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/React-Amplify-Cloud9-AMI-Lambda/task5.png)
    1. Explore the code in <App />:
        1. To be able to use effects, you **import useEffect** from react at the top of the file.
        1. A new state, instances, defaults to an empty array. This mimics a real-life scenario where on page load, you do not know the state of your instances.
        1. An useEffect is implemented that sets the instances state to an array containing mock EC2 instance data. This mimics a real-life scenario where you load the page first, then query an API to get EC2 data.
        1.  Note: The array at the end of the useEffect is the effect’s dependencies. If any variable listed in this array changes, the effect runs. Having an empty dependency list, i.e. an empty array, means that the effect only runs when the component first loads.
        1.  **Knowledge check**: Can you think of a scenario where the dependency list is not empty?
        1. **Answer** 
            1. The **InstanceRows** function is a reusable functional component that iterates through the instances array and returns table rows.
            1. The **launchInstance and terminateInstance** functions updates the instances state with a new row or removes a row based on the ID, respectively.

## Task 6: Create an Amplify app using the CLI and add authentication services
1. Have built a frontend with mock data. Let’s begin adding AWS services to your app.
1. Install the **Amplify CLI**, create an Amplify app with an **AWS Identity and Access Management (IAM)** permissions boundary, and add authentication services based on **Amazon Cognito**.
1. A **permissions boundary** is an IAM feature that helps centralized cloud IAM teams to safely empower application developers to create new IAM roles and policies in AWS. 
1. Instead of logging tickets to your cloud team to create roles on your behalf, you are allowed to create roles yourselves, but only if you attach a permissions boundary to the role. 
1. This increases your development agility and allows you to bring features to market faster.
1. **Amazon Cognito** lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. 
1. **Amazon Cognito** scales to millions of users and supports sign-in with social identity providers, such as Apple, Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0 and OpenID Connect.
1. Run the following command to install the Amplify CLI:
    ```
    npm install -g @aws-amplify/cli
    ```
    1. Note: The -g flag tells npm to install the package globally, instead of at the project level.
1. Next you create the Amplify application using CLI commands. The first command is used to create a variable for the ARN of a permissions boundary policy with AmplifyPermissionBoundary in the name. Then it changes directories into the ec2-vendor directory and issues the command to create the Amplify application.
1. **Note:** For security purposes this requires specifying a permissions boundary policy. Since the creation of a role and an inline policy is required as part of creating the Amplify application, this permissions boundary policy restricts the permissions that can be used in the inline policy to ensure security of the lab.
1. Run the following commands to create the Amplify app:
    ```
    policy_arn=$(aws iam list-policies --scope Local --query 'Policies[?contains(PolicyName, `AmplifyPermissionBoundary`)].Arn | [0]' --output text) && echo -e "\n Policy ARN: $policy_arn\n"
    
    cd ~/environment/ec2-vendor
    amplify init --permissions-boundary $policy_arn
    ```    
    1. Accept the default for the project name: ec2vendor.
    1. Answer Y to initialize the project with the predefined configuration.
    1. Use AWS profile for authentication.
    1. Select the default profile.
1. Run the following command to add authentication services to the Amplify app:
1. **Note:** If at any time you select or input the wrong option, you can use ctrl + c to break out of the CLI and attempt it again.
    ```
    amplify add auth
    ```
    1. Select Default configuration to use the preconfigured default.
    1. Select Username as the method of signing in.
    1. Select No, I am done. to complete the configuration.
1. Run the following command to deploy authentication services to AWS:
    ```
    amplify push --yes
    ```
1. Explore the resources created by Amplify:
    1. In **AWS Cloud9**, in the project’s **src** directory, open **aws-exports.js**. This file is generated by Amplify and allows the frontend easy access to the backend by referencing specific IDs.
    1. In your browser, switch to the **AWS Cloud9** tab. Use the **AWS search bar** to search for **Cognito**. Choose the service from the list of results.
    1. If available, select the Try out the new interface link.
    1. In the user pools list, select the ec2vendor user pool. After a user signs up, their information appears here.

## Task 7: Update the app to use authentication
Now that you have AWS authentication services configured on the backend, you use the aws-amplify library to link the frontend with the backend. This library provides a declarative interface across different categories of operations in order to make common tasks easier to add into your application.

In this task, you update your app with the authentication capabilities.

Switch back to the browser tab open to the Cloud9-Lab-IDE environment.

 Command: Run the following commands to install the aws-amplify library:


cd ~/environment/ec2-vendor
npm install aws-amplify
 Note: The warnings can be ignored.

 Expected output: Output has been truncated.


******************************
**** This is OUTPUT ONLY. ****
******************************

up to date, audited 2680 packages in 6s
 Command: Run the following command to update your code to use authentication:

cp ~/environment/lab-resources/frontend/example5/* ~/environment/ec2-vendor/src
 Expected output:

None, unless an error occurs.

 Refresh the preview.

You are now prompted with a sign in screen. To create an account, select the Create Account tab

Input a username, password and valid email address
Choose Create Account
Wait for the validation code to arrive at your email, and enter the code at the next screen
Choose Confirm. You are now taken to the app.
 Note: It may take a few minutes for the verification code to arrive at your email address.

 Consider: Explore the app:

There is a text box that shows the user object. This object is passed in from the Amplify authenticator component and includes user-level attributes as well as session details, such as the JWT token. This object is typically used programmatically and is only shown here for learning purposes.

The Sign Out button is functional and signs you out of the app.

 Consider: Explore the code in <App />:

You use a combination of the aws-amplify, Amplify UI libraries and aws-exports.js to implement authentication.

withAuthenticator is a higher order component that wraps around the App component. If you are not authenticated, then the app is hidden and the authentication component is shown. If you are authenticated, then the app is shown and the authenticator passes in the user object and the sign out function to App.

 Note: There are other options for using the authentication component directly or even writing your own components.

The table component has been moved into its own file, named InstanceTable.jsx. It takes in props and returns the entirety of the instance table.
 Task complete: You successfully updated the app to use authentication.

Task 8: Create an AWS Lambda function
AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers. You can trigger Lambda from over 200 AWS services and software as a service (SaaS) applications, and only pay for what you use.

In this task, you use Amplify to create a Node.js Lambda function that takes input from an API and interact with the EC2 service using the AWS SDK. This includes getting Amazon Machine Image (AMI) data, querying, starting and terminating EC2 instances.

TASK 8.1: CREATE THE FUNCTION
 Command: Run the following command to add a Lambda function via Amplify:

cd ~/environment/ec2-vendor
amplify add function
Select Lambda function (serverless function) for the capability.
Enter ec2VendorApi for the function name.
Select NodeJS for the runtime.
Select the Hello World template.
Answer N to configuring advanced settings and press Enter.
Answer N to editing the local Lambda function now and press Enter.
 Expected output:


? Select which capability you want to add: Lambda function (serverless function)
? Provide an AWS Lambda function name: ec2VendorApi
? Choose the runtime that you want to use: NodeJS
? Choose the function template that you want to use: Hello World

Available advanced settings:
- Resource access permissions
- Scheduled recurring invocation
- Lambda layers configuration
- Environment variables configuration
- Secret values configuration

? Do you want to configure advanced settings? No
? Do you want to edit the local lambda function now? No
Successfully added resource ec2VendorApi locally.

Next steps:
Check out sample function code generated in <project-dir>/amplify/backend/function/ec2VendorApi/src
"amplify function build" builds all of your functions currently in the project
"amplify mock function <functionName>" runs your function locally
To access AWS resources outside of this Amplify app, edit the /home/ec2-user/environment/ec2-vendor/amplify/backend/function/ec2VendorApi/custom-policies.json
"amplify push" builds all of your local backend resources and provisions them in the cloud
"amplify publish" builds all of your local backend and front-end resources (if you added hosting category) and provisions them in the cloud
 Command: The Lambda code has been pre-written. Run the following command to update your local Lambda code:

cp -rp ~/environment/lab-resources/backend/ec2VendorApi/* ~/environment/ec2-vendor/amplify/backend/function/ec2VendorApi
 Expected output:

None, unless an error occurs.

 Consider: Explore the code in the ec2VendorApi Lambda function:

In the AWS Cloud9 file explorer, navigate to the following path: ec2-vendor/amplify/backend/function/ec2VendorApi. This is where the local Lambda function is located.

custom-policies.json contains IAM statements that are given to the Lambda function execution role.

 Note: Remember the IAM permissions boundary you specified when you ran amplify init? This permissions boundary is applied to every role that Amplify creates, including this Lambda execution role.

The src directory contains the Lambda function code. The exported handler function in index.js is the entry into the function.

Your example function contains simple conditional statements to call functions depending on the input:

GET HTTP method
/vendor/ami: query EC2 for Amazon Linux 2 AMIs based on the architecture
/vendor/instance: query EC2 for running EC2 instances based on the owner tag
POST HTTP method
/vendor/instance/launch: launch an EC2 instance with appropriate tagging
/vendor/instance/terminate: terminate an EC2 instance given the instance ID
The function always returns a JSON with an HTTP status code of 200 and a variable body from the EC2 API output.

The src/event.json file provides a sample input that you can use to test the function locally. This is covered in the next subtask.

TASK 8.2: TEST THE FUNCTION LOCALLY
In order to quickly test and debug without pushing all changes in your project to the cloud, Amplify supports Local Mocking and Testing for certain categories, including AWS Lambda.

 Command: Before you can test the Lambda function locally, you must install the function’s dependencies. Run the following command to do so:

cd ~/environment/ec2-vendor/amplify/backend/function/ec2VendorApi/src/
npm install aws-sdk
 Expected output: Output is truncated.


******************************
**** This is OUTPUT ONLY. ****
******************************

added 32 packages, and audited 33 packages in 3s
 Command: Run the following command to test the AMI query logic of the Lambda function:

amplify mock function ec2VendorApi --event "src/event.json"
 Expected output: Output has been truncated.


Ensuring latest function changes are built...
Starting execution...
Result:
{
  "statusCode": 200,
  "headers": {
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Headers": "*"
  },
  "body": "{\"Images\":[{\"Architecture\":\"x86_64\",...\"Tags\":[],\"VirtualizationType\":\"hvm\",\"DeprecationTime\":\"2023-07-27T06:22:54.000Z\"}]}"
}
Finished execution.
 Task complete: You successfully created an AWS Lambda function.

Task 9: Create a secure REST API backed by an AWS Lambda function
Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the “front door” for applications to access data, business logic, or functionality from your backend services.

In this task, you use Amplify to create a secure REST API powered by Amazon API Gateway. This is backed by the Lambda function you created in the previous task.

 Command: Run the following command to add a secure REST API via Amplify:

cd ~/environment/ec2-vendor
amplify add api
Select REST for the API type.
Enter vendorApi for the API friendly name.
Enter /vendor for the path.
Use the arrow down key to select Use a Lambda function already added in the current Amplify project for the Lambda source. Since you only have one Lambda function, the CLI selects this automatically.
Answer Y to restrict API access.
Select Authenticated users only for who should have access.
Use ctrl + a to grant all options to authenticated users.
Answer N to not add another path.
 Expected output:


? Select from one of the below mentioned services: REST
âœ” Provide a friendly name for your resource to be used as a label for this category in the project: Â· vendorApi
âœ” Provide a path (e.g., /book/{isbn}): Â· /vendor
âœ” Choose a Lambda source Â· Use a Lambda function already added in the current Amplify project
Only one option for [Choose the Lambda function to invoke by this path]. Selecting [ec2VendorApi].
âœ” Restrict API access? (Y/n) Â· yes
âœ” Who should have access? Â· Authenticated users only
âœ” What permissions do you want to grant to Authenticated users? Â· create, read, update, delete
âœ” Do you want to add another path? (y/N) Â· no
âœ… Successfully added resource vendorApi locally

âœ… Some next steps:
"amplify push" will build all your local backend resources and provision it in the cloud
"amplify publish" will build all your local backend and frontend resources (if you have hosting category added) and provision it in the cloud
 Command: Run the following command to deploy the secure REST API and Lambda function to AWS:

amplify push --yes
 Expected output:


âœ” Successfully pulled backend environment dev from the cloud.

    Current Environment: dev

â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ Category â”‚ Resource name     â”‚ Operation â”‚ Provider plugin   â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Function â”‚ ec2VendorApi      â”‚ Create    â”‚ awscloudformation â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Api      â”‚ vendorApi         â”‚ Create    â”‚ awscloudformation â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Auth     â”‚ ec2vendora1b2c3d4 â”‚ No Change â”‚ awscloudformation â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
âš ï¸ Warning: You've specified "*" as the "Resource" in ec2VendorApi's custom IAM policy.
 This will grant ec2VendorApi the ability to perform ec2:DescribeImages,ec2:DescribeInstances on ALL resources in this AWS Account.

Deployment completed.
Deployed root stack ec2vendor [ ======================================== ] 4/4
        amplify-ec2vendor-dev-173538   AWS::CloudFormation::Stack     UPDATE_COMPLETE
        functionec2VendorApi           AWS::CloudFormation::Stack     CREATE_COMPLETE
        authec2vendora1b2c3d4          AWS::CloudFormation::Stack     UPDATE_COMPLETE
        apivendorApi                   AWS::CloudFormation::Stack     CREATE_COMPLETE
Deployed function ec2VendorApi [ ======================================== ] 4/4
        LambdaExecutionRole            AWS::IAM::Role                 CREATE_COMPLETE
        LambdaFunction                 AWS::Lambda::Function          CREATE_COMPLETE
        CustomLambdaExecutionPolicy    AWS::IAM::Policy               CREATE_COMPLETE
        lambdaexecutionpolicy          AWS::IAM::Policy               CREATE_COMPLETE
Deployed api vendorApi [ ======================================== ] 5/5
        vendorApi                      AWS::ApiGateway::RestApi       CREATE_COMPLETE
        vendorApiDefault5XXResponse    AWS::ApiGateway::GatewayRespoâ€¦ CREATE_COMPLETE
        vendorApiDefault4XXResponse    AWS::ApiGateway::GatewayRespoâ€¦ CREATE_COMPLETE
        functionec2VendorApiPermissioâ€¦ AWS::Lambda::Permission        CREATE_COMPLETE
        DeploymentAPIGWvendorApi9cf8dâ€¦ AWS::ApiGateway::Deployment    CREATE_COMPLETE

REST API endpoint: https://a1b2c3d4e5.execute-api.us-west-2.amazonaws.com/dev
 Note: The following warning can be safely ignored.


ðŸ›‘ Failed to upload AMPLIFY_function_ec2VendorApi_deploymentBucketName to ParameterStore
 Consider: In the terminal, select the URL for the REST API endpoint and select Open. This opens the API URL in a new window. You should get a message of Missing Authentication Token. This is because you chose to create a secure API that uses IAM authentication. This prevents anonymous access to the API. You can close this new tab.

 Task complete: You successfully created a secure REST API backed by an AWS Lambda function.

Task 10: Update the app to use the API to query Amazon Linux 2 AMI data
Before you implement launching and terminating EC2 instances, let’s implement something simpler: querying Amazon Machine Images (AMIs) from the frontend using the API and Lambda function.

An Amazon Machine Image (AMI) is a supported and maintained image provided by AWS that provides the information required to launch an instance.

In this task, you update your app to interact with the secure REST API and query for Amazon Linux 2 AMI data based on x86_64 or arm64 architecture.

 Command: Run the following command to update your code to use the REST API:

cp ~/environment/lab-resources/frontend/example6/* ~/environment/ec2-vendor/src
 Expected output:

None, unless an error occurs.

 Refresh the preview.
 Expected output:

Updated Amplify interface

The image depicts the Amplify app interface that welcomes the logged in user by their email address. Below this is a toggle switch that lets you pick between x86_64 or arm64 architectures for an AMI. To the right of this toggle switch is a button labeled Get Amazon Linux 2 AMI Data and to the right of this button is a Sign Out button. Below this is a window with a title of AMI data that will be populated with AMI details in JSON format when you choose the Get Amazon Linux 2 AMI Data button.

 Consider: Explore the app:

There is a toggle switch that lets you pick between x86_64 or arm64 architectures.
 Knowledge check: What is causing the toggle button text to switch between x86_64 and arm64?

Answer
 Consider: Explore the code in <App />:

You import the API interface from the aws-amplify library to interact with the API. Behind the scenes, the interface is using an HTTP client library to execute the HTTP requests.

An async function enables asynchronous, promise-based behavior. handlGetAmi is an async function that:

Sets setIsLaunching to true. This causes the button to become disabled and displays the spinning circle, which provides feedback to the user that the app is doing something.

Calls the REST API with a GET request using the /vendor/ami path. This returns a JSON object of AMI data, same as your Lambda test in the previous task.

Sets the amiData state to the JSON object. This causes the app to rerender and display the JSON object in the text field.

Sets setIsLaunching back to false.

getLatestAmiId is a helper function that parses through the AMI JSON object and returns the latest AMI ID. You use this to decide which AMI to launch your EC2 instances from in the next task.

 Task complete: You successfully updated the app to use the API to query Amazon Linux 2 AMI data.

Task 11: Update the app to launch and terminate Amazon EC2 instances
In this task, you update your app to launch and terminate EC2 instances.

 Command: Run the following command to update your app:

cp ~/environment/lab-resources/frontend/example7/* ~/environment/ec2-vendor/src
 Expected output:

None, unless an error occurs.

 Refresh the preview.
 Consider: Explore the app:

The instance list state is initially empty. When the page loads, the instance list is refreshed.

The Launch button launches an EC2 instance and returns EC2 metadata. The EC2 instance is also tagged with the user who launched it.

The Terminate button becomes active when the EC2 instance is in a running state. Selecting this button terminates the instance.

 Consider: Explore the code in <App />:

refreshInstances, handleLaunchInstance, and handleTerminateInstance have the same general concept:

Set the “loading” state to true.
Call the REST API to refresh instances, or launch or terminate an instance.
Set the “loading” state to false.
On initial page load, the instance list is refreshed. This is because you call refreshInstances in an useEffect hook.

 Task complete: You successfully updated the app to launch and terminate Amazon EC2 instances.

Task 12: Host the app on Amazon CloudFront
AWS Amplify Hosting is a fully managed CI/CD and hosting service for fast, secure, and reliable static and server-side rendered apps that scale with your business. Your app is deployed to Amazon CloudFront’s content delivery network (CDN) with hundreds of points of presence globally.

In this task, you use Amplify to add hosting and deploy your app to Amazon CloudFront.

 Command: Run the following command to add hosting via Amplify:

cd ~/environment/ec2-vendor
amplify add hosting
Select Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment) for the module to execute.
Select Manual deployment for the type.
 Expected output:


âœ” Select the plugin module to execute Â· Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment)
? Choose a type Manual deployment

You can now publish your app using the following command:

Command: amplify publish
 Command: Run the following command to publish the app. During this process, Amplify creates an optimized build of your app for hosting.

amplify publish --yes
 Expected output:


âœ” Successfully pulled backend environment dev from the cloud.

    Current Environment: dev

â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ Category â”‚ Resource name     â”‚ Operation â”‚ Provider plugin   â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Hosting  â”‚ amplifyhosting    â”‚ Create    â”‚ awscloudformation â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Auth     â”‚ ec2vendor1e2344a8 â”‚ No Change â”‚ awscloudformation â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Function â”‚ ec2VendorApi      â”‚ No Change â”‚ awscloudformation â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¼â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ Api      â”‚ vendorApi         â”‚ No Change â”‚ awscloudformation â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜

Deployment completed.
Deploying root stack ec2vendor [ ================================-------- ] 4/5
        amplify-ec2vendor-dev-210943   AWS::CloudFormation::Stack     UPDATE_IN_PROGRESS             Wed Mar 22 2023 21:36:58â€¦
        authec2vendor1e2344a8          AWS::CloudFormation::Stack     UPDATE_COMPLETE                Wed Mar 22 2023 21:37:05â€¦
        hostingamplifyhosting          AWS::CloudFormation::Stack     CREATE_COMPLETE                Wed Mar 22 2023 21:37:17â€¦
        functionec2VendorApi           AWS::CloudFormation::Stack     UPDATE_COMPLETE                Wed Mar 22 2023 21:37:06â€¦
        apivendorApi                   AWS::CloudFormation::Stack     UPDATE_COMPLETE                Wed Mar 22 2023 21:37:09â€¦
Deployed hosting amplifyhosting [ ======================================== ] 1/1

Deployment state saved successfully.


Publish started for amplifyhosting

> ec2-vendor@0.1.0 build
> react-scripts build

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  261.32 kB  build/static/js/main.f356e6ca.js
  33.01 kB   build/static/css/main.746f56ac.css

The project was built assuming it is hosted at /.
You can control this with the homepage field in your package.json.

The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build

Find out more about deployment here:

  https://cra.link/deployment

âœ” Zipping artifacts completed.
âœ” Deployment complete!
https://dev.dryf2k7c2wzr.amplifyapp.com
To access the public facing app, copy and paste the URL from the published output into a new tab.

You are taken to the authentication page, log in with the user name and password you used earlier.

 Note: You are allowed spin up to 2 instances in this lab. Any additional instances will be deleted automatically.

 Task complete: You successfully hosted the app on Amazon CloudFront.

Task 13: Cleanup
When you are done with resources you are no longer going to use, it is always best to remove those resources to keep from incurring charges.

In this task, you use the AWS Amplify CLI to remove all resources you built during the lab: Cognito user and identity pools, API Gateway REST API, Lambda function and CloudFront hosting.

 Note: After this task, the app no longer functions.

 Command: Run the following command to update your app:

cd ~/environment/ec2-vendor
amplify delete
Answer Y to delete all Amplify resources.
 Expected output:


âœ” Are you sure you want to continue? This CANNOT be undone. (This will delete all the environments of the project from the cloud and wipe out all the local files created by Amplify CLI) (y/N) Â· yes
â ‹ Deleting resources from the cloud. This will take a few minutes.
Deleting env: dev.
âœ” Project deleted in the cloud.
âœ… Project deleted locally.
 Task complete: You successfully used the AWS Amplify CLI to remove all resources you built during the lab.

Conclusion
 Task complete: You now have successfully:

Created a full-stack web app on AWS.
Implemented React concepts such as states, effects and props.
Created auth, API, function, and hosting resources on AWS using AWS Amplify.
Integrated your frontend with backend services.
The architecture that is built during this lab is as follows:

Architecture Diagram

The diagram depicts the use of the following resources:

Amazon Cognito used to sign up/sign in to the app
Amazon CloudFront
Amazon S3
Amazon API Gateway
AWS Lambda
Amazon EC2
Amazon VPC
AWS IAM
AWS Cloud9
AWS Amplify
You used AWS Amplify to minimize heavy lifting and created a first iteration of a PoC. By using Amplify, you gain development agility and can focus more on business needs.
