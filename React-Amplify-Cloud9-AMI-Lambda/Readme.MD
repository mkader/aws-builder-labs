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
    1. Accept the default for the project name: **ec2vendor**.
    1. Answer **Y** to initialize the project with the predefined configuration.
    1. Use **AWS profile** for authentication.
    1. Select the **default** profile.
1. Run the following command to add authentication services to the Amplify app:
1. **Note:** If at any time you select or input the wrong option, you can use ctrl + c to break out of the CLI and attempt it again.
    ```
    amplify add auth
    ```
    1. Select **Default configuration** to use the preconfigured default.
    1. Select **Username** as the method of signing in.
    1. Select **No, I am done**. to complete the configuration.
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
1. Now that you have AWS authentication services configured on the backend, you use the aws-amplify library to link the frontend with the backend. 
1. This library provides a declarative interface across different categories of operations in order to make common tasks easier to add into your application.
1. update your app with the authentication capabilities.
1. Switch back to the browser tab open to the **Cloud9-Lab-IDE** environment.
1. Run the following commands to install the aws-amplify library:
    ```
    cd ~/environment/ec2-vendor
    npm install aws-amplify
    ```
1. Run the following command to update your code to use authentication:
    ```
    cp ~/environment/lab-resources/frontend/example5/* ~/environment/ec2-vendor/src
    ```
1.Refresh the preview. Prompted with a sign in screen. To **create an account**, select the Create Account tab
1. Explore the code in &lt;App />:
    1. You use a combination of the aws-amplify, Amplify UI libraries and aws-exports.js to implement authentication.
    1. withAuthenticator is a higher order component that wraps around the App component. If you are not authenticated, then the app is hidden and the authentication component is shown. If you are authenticated, then the app is shown and the authenticator passes in the user object and the sign out function to App.
    1. The table component has been moved into its own file, named InstanceTable.jsx. It takes in props and returns the entirety of the instance table.


## Task 8: Create an AWS Lambda function
1. use Amplify to create a **Node.js Lambda function** that takes input from an API and interact with the EC2 service using the AWS SDK. This includes getting **Amazon Machine Image (AMI)** data, querying, starting and terminating EC2 instances.
### TASK 8.1: CREATE THE FUNCTION
1. Run the following command to add a Lambda function via Amplify:
    ```
    cd ~/environment/ec2-vendor
    amplify add function
    ```
    1. Select **Lambda function (serverless function)** for the capability.
    1. Enter **ec2VendorApi** for the function name.
    1. Select **NodeJS** for the runtime.
    1. Select the **Hello World** template.
    1. Answer **N** to configuring advanced settings and press **Enter**.
    1. Answer **N** to editing the local Lambda function now and press **Enter**.
1.The Lambda code has been pre-written. Run the following command to update your local Lambda code:
```
cp -rp ~/environment/lab-resources/backend/ec2VendorApi/* ~/environment/ec2-vendor/amplify/backend/function/ec2VendorApi
```
1. Explore the code in the ec2VendorApi Lambda function:
    1. In the AWS Cloud9 file explorer, navigate to the following path: ec2-vendor/amplify/backend/function/ec2VendorApi. This is where the local Lambda function is located.
    1. custom-policies.json contains IAM statements that are given to the Lambda function execution role.
    1.  Note: Remember the IAM permissions boundary you specified when you ran amplify init? This permissions boundary is applied to every role that Amplify creates, including this Lambda execution role.
    1. The src directory contains the Lambda function code. The exported handler function in index.js is the entry into the function.
    1. Your example function contains simple conditional statements to call functions depending on the input:
        1. GET HTTP method
            1. /vendor/ami: query EC2 for Amazon Linux 2 AMIs based on the architecture
    /vendor/instance: query EC2 for running EC2 instances based on the owner tag
        1. POST HTTP method
            1./vendor/instance/launch: launch an EC2 instance with appropriate tagging
            1. /vendor/instance/terminate: terminate an EC2 instance given the instance ID
    1. The function always returns a JSON with an HTTP status code of 200 and a variable body from the EC2 API output.
    1. The src/event.json file provides a sample input that you can use to test the function locally. This is covered in the next subtask.

### TASK 8.2: TEST THE FUNCTION LOCALLY
1. In order to quickly test and debug without pushing all changes in your project to the cloud, Amplify supports Local Mocking and Testing for certain categories, including AWS Lambda.
1. Before you can test the Lambda function locally, you must install the function’s dependencies. Run the following command to do so:
    ```
    cd ~/environment/ec2-vendor/amplify/backend/function/ec2VendorApi/src/
    npm install aws-sdk
    ```
1. Run the following command to test the AMI query logic of the Lambda function:
    ```
    amplify mock function ec2VendorApi --event "src/event.json"
    ```

## Task 9: Create a secure REST API backed by an AWS Lambda function
1. **Amazon API Gateway** is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the “front door” for applications to access data, business logic, or functionality from your backend services.
1. In this task, you use Amplify to create a secure REST API powered by Amazon API Gateway. This is backed by the Lambda function you created in the previous task.
1. run the following command to add a secure REST API via Amplify:
    ```
    cd ~/environment/ec2-vendor
    amplify add api
    ```
    1. Select **REST** for the API type.
    1. Enter **vendorApi** for the API friendly name.
    1. Enter **/vendor** for the path.
    1. Use the arrow down key to select **Use a Lambda function already added in the current. Amplify project** for the Lambda source. Since you only have one Lambda function, the 
    CLI selects this automatically.
    1. Answer **Y** to restrict API access.
    1. Select Authenticated users** only for who should have access.
    1. Use **ctrl + a** to grant all options to authenticated users.
    1. Answer **N** to not add another path.
1. Run the ommand to deploy the secure REST API and Lambda function to AWS:
    ```
    amplify push --yes
    ```
1. select the URL for the REST API endpoint and select Open. This opens the API URL in a new window. You should get a message of Missing Authentication Token. This is because you chose to create a secure API that uses IAM authentication. This prevents anonymous access to the API. You can close this new tab.

## Task 10: Update the app to use the API to query Amazon Linux 2 AMI data
1. **Amazon Machine Images (AMIs)** from the frontend using the API and Lambda function.
1. An **Amazon Machine Image (AMI)** is a supported and maintained image provided by AWS that provides the information required to launch an instance.
1. Update your app to interact with the secure REST API and query for **Amazon Linux 2 AMI** data based on x86_64 or arm64 architecture.
1. Run the following command to update your code to use the REST API:
    ```
    cp ~/environment/lab-resources/frontend/example6/* ~/environment/ec2-vendor/src
    ```
1. Refresh the preview.
![img](https://raw.githubusercontent.com/mkader/aws-builder-labs/main/React-Amplify-Cloud9-AMI-Lambda/toggle.png)
1. Explore the code in <App />:
    1. You import the API interface from the aws-amplify library to interact with the API. Behind the scenes, the interface is using an HTTP client library to execute the HTTP requests.
    1. An async function enables asynchronous, promise-based behavior. handlGetAmi is an async function that:
        1. Sets setIsLaunching to true. This causes the button to become disabled and displays the spinning circle, which provides feedback to the user that the app is doing something.
        1. Calls the REST API with a GET request using the /vendor/ami path. This returns a JSON object of AMI data, same as your Lambda test in the previous task.
        1. Sets the amiData state to the JSON object. This causes the app to rerender and display the JSON object in the text field.
        1. Sets setIsLaunching back to false.
    1. getLatestAmiId is a helper function that parses through the AMI JSON object and returns the latest AMI ID. You use this to decide which AMI to launch your EC2 instances from in the next task.

## Task 11: Update the app to launch and terminate Amazon EC2 instances
1. update your app to launch and terminate EC2 instances.
1. Run the following command to update your app:
    ```
    cp ~/environment/lab-resources/frontend/example7/* ~/environment/ec2-vendor/src
    ```
1. Refresh the preview.
1. Explore the code in <App />:
    1. refreshInstances, handleLaunchInstance, and handleTerminateInstance have the same general concept:
        1. Set the “loading” state to true.
        1. Call the REST API to refresh instances, or launch or terminate an instance.
        1. Set the “loading” state to false.
    1. On initial page load, the instance list is refreshed. This is because you call refreshInstances in an useEffect hook.

## Task 12: Host the app on Amazon CloudFront
1. AWS Amplify Hosting is a fully managed CI/CD and hosting service for fast, secure, and reliable static and server-side rendered apps that scale with your business. 
1. Your app is deployed to **Amazon CloudFront’s content delivery network (CDN)** with hundreds of points of presence globally.
1. use Amplify to add hosting and deploy your app to Amazon CloudFront.
1. Run the following command to add hosting via Amplify:
    ```
    cd ~/environment/ec2-vendor
    amplify add hosting
    ```
    1. Select **Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment)** for the module to execute.
    1. Select **Manual deployment** for the type.
1. Run the command to publish the app. During this process, Amplify creates an optimized build of your app for hosting.
    ```
    amplify publish --yes
    ```
1. To access the public facing app, copy and paste the URL from the published output into a new tab.
1. You are taken to the authentication page, log in with the user name and password you used earlier.

## Task 13: Cleanup
1. When you are done with resources you are no longer going to use, it is always best to remove those resources to keep from incurring charges.
1. use the **AWS Amplify CLI** to remove all resources you built during the lab: Cognito user and identity pools, API Gateway REST API, Lambda function and CloudFront hosting.
1. Run the following command to update your app:
```
cd ~/environment/ec2-vendor
amplify delete
```
    1. Answer **Y** to delete all Amplify resources.

## Architecture Diagram
1. The diagram depicts the use of the following resources:
    1. Amazon Cognito used to sign up/sign in to the app
    1. Amazon CloudFront
    1. Amazon S3
    1. Amazon API Gateway
    1. AWS Lambda
    1. Amazon EC2
    1. Amazon VPC
    1. AWS IAM
    1. AWS Cloud9
    1. AWS Amplify
1. You used AWS Amplify to minimize heavy lifting and created a first iteration of a PoC. 
1. By using Amplify, you gain development agility and can focus more on business needs.
