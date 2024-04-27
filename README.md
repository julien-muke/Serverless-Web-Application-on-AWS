# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Build a serverless web application using AWS Lambda, DynamoDB, and S3.


## <a name="introduction">🤖 Introduction</a>

In this project, you will build a serverless web application using AWS Lambda, DynamoDB, and S3. The application will allow users to create, read, update, and delete (CRUD) items from a DynamoDB table.


## <a name="design">📐 Project Architecture</a>

![Serverless Web Application on AWS](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/9c7fefe7-014e-4648-a105-fc67b201d882)


## <a name="steps">☑️ Steps</a>

* Create a DynamoDB table to store the items
* Build a Lambda function to handle the CRUD operations on the DynamoDB table
* Use S3 to store and host the web application's static files (HTML, CSS, and JavaScript)
* Create a CloudFront distribution to serve the S3-hosted static files with low latency


## ➡️ Step 1 - Create an Amazon S3 Bucket

First, you need to create an Amazon S3 bucket where you will store your objects.

1. Sign in to the AWS Management Console and open the Amazon S3 console at https://console.aws.amazon.com/s3/
2. In the navigation bar on the top of the page, choose the name of the currently displayed AWS Region. Next, choose the Region in which you want to create a bucket. 
3. In the left navigation pane, choose Buckets.
4. Choose Create bucket.

![Screenshot 2024-04-11 at 18 24 10](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/d97d4214-bcb5-4d90-a4f6-cdc3410f095f)


5. Under General configuration, view the AWS Region where your bucket will be created.
6. Under Bucket type, choose General purpose.
7. For Bucket name, enter a name for your bucket, i will name it `jmwebapp`

![Create-S3-bucket-S3-us-east-1 (1)(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/16d52182-f605-4c69-a885-153b3dfe8ce2)

8. Scroll down and leave everything else as default and Choose Create bucket.

You've created a bucket in Amazon S3. 

After creating a bucket in Amazon S3, you're ready to upload an object to the bucket. An object can be any kind of file: a text file, a photo, a video, and so on. 

1. In the Buckets list, choose the name of the bucket that you want to upload your object to.

![Screenshot 2024-04-11 at 18 29 50](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/c0ff0d6d-07bd-48d1-84cb-9df4fb391e4e)

2. On the Objects tab for your bucket, choose Upload.

![Screenshot 2024-04-11 at 18 30 06](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/a761de2e-4eab-446a-8343-67ae885539a0)


3. Under Files and folders, choose Add files.

4. Choose a file to upload, we are going to upload the code provided in this GitHub repo, file name: `index.html` `script.js` `style.css`, and then choose Open, and choose Upload.

![Screenshot 2024-04-12 at 11 34 37](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/30b21caa-5a6e-4f37-8e36-4a2e0333625e)


You've successfully uploaded an object to your bucket. 

## ➡️ Step 2 - Create a CloudFront distribution

CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the request is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.

To create a distribution:

1. On Console and open the CloudFront console at https://console.aws.amazon.com/cloudfront/v4/home.
2. In the navigation pane, choose Distributions, then choose Create distribution.

![Screenshot 2024-04-11 at 18 40 59](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/60a5384f-1a8a-4621-b20a-954724e3a403)


3. The origin domain is the DNS domain name of the Amazon S3 bucket or HTTP server from which you want CloudFront to get objects for this origin, for example: Amazon S3 bucket – DOC-EXAMPLE-BUCKET.s3.us-west-2.amazonaws.com 

We've created an S3 bucket earlier called `jmwebapp`. Our origin domain is going to be `jmwebapp.s3.us-east-1.amazonaws.com`

4. Origin path is optional, we will keep that as default.

![CloudFront-Global(3)(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/ba422e1c-33b1-4641-b3ca-22aa6a64444b)


5. Choose Origin access control settings (recommended) if you want to make it possible to restrict access to an Amazon S3 bucket origin to only specific CloudFront distributions.

Choose Public if the Amazon S3 bucket origin is publicly accessible.


![Screenshot 2024-04-11 at 18 45 58](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/2f9949af-03ab-41e9-ac9d-d0c3fc4f2126)

6. Create a Origin Access Control  - OAC

![Screenshot 2024-04-11 at 18 46 31](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/e3153d79-22f4-49bb-9c5e-9600c97cf73d)


7. You can use AWS WAF to protect your CloudFront distributions and origin servers. AWS WAF is a web application firewall that helps secure your web applications and APIs by blocking requests before they reach your servers.

But for the sake of this tutorial, we'll keep it simple and not use AWS WAF, choose "Do not use security protection"

![Screenshot 2024-04-11 at 18 50 10](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/77933474-9332-4eb3-bb2a-e17e394aac1d)

8. After CloudFront creates your distribution, the value of the `Status` column for your distribution will change from `Deploying` to the date and time that the distribution is deployed. If you chose to enable the distribution, it will be ready to process requests at this time. 

9.  Scroll down, leave everything else as default, and click "Create distribution"

![Screenshot 2024-04-18 at 14 43 19](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/b20eb5d0-30f9-42aa-a2da-f2e0005703a3)


9. When your distribution is deployed, confirm that you can access your content using your new CloudFront URL or CNAME. For more information, see Testing a distribution.


Next, you need to copy the S3 Bucket policy, which you need to attach to your bucket so that the CloudFront can access your AWS S3 bucket: 

* Back to the your distribution, click on Origin Tab, elect the origin name that you have created just and click on "Edit"

![Screenshot 2024-04-18 at 14 47 25](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/b495c7dd-06e3-46b5-b310-0c32ae7611cc)


* Scroll down until you see the CloudFront permission policy statement, just click on you know "Copy policy" button and the policy will be copied to your clipboard, you need to paste this in your Amazon S3 bucket.


![Screenshot 2024-04-18 at 14 48 31](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/5fc36379-96df-4b72-b36e-99900248802e)


* Go back to your S3 Bucket and select the bucket where you have you know stored everything.
* Go to permission Tab, under permission you will see "Bucket policy", select that and click "Edit"

![Screenshot 2024-04-18 at 14 52 19](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/b57ba7b0-c396-49f3-85df-777a923dd6fc)


* Then you paste the content that you have you know copied. Basically the policy code is just allowing cloudfront service principle to access your S3 bucket, then click "Save changes"


![Edit-bucket-policy-S3-bucket-jmwebapp-S3-us-east-1](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/16438cbf-e4b4-4a2c-b604-1ef95c41bc17)


* On the distribution console, we need to change the default root object to `index.html` because our AWS bucket contains a file called index.html and that is the entry point for our web application.

![Screenshot 2024-04-18 at 15 01 01](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/f12fee38-1ac4-4290-95d2-2cf72e653308)


* The default root object is the oject (file name) to return when a viewer requests the URL (/) instead of a specific oject.

After you make the changes, make sure that it is deployed successfully.

![CloudFront-Global(2)(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/96423495-ca2e-4f2d-a4c0-b6d24bc00625)


## ➡️ Step 3 - Enable Route 53 for the CloudFront distribution

We need to setup AWS Route 53, you need to have valid domain name. You can purchase one either from AWS Route 53 or other hositng provider.

I have a domain name `julienmuke.cloud` form Hostinger (https://www.hostinger.com/)

1. Let's Create Hosted Zone

A hosted Zone one tells Route 53 how to respond to DNS queries for a domain such as example.com or julienmuke.cloud.

* Back to your console navigate to AWS Route 53, choose Hosted zones in the navigation pane

![Screenshot 2024-04-18 at 15 12 56](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/53fa6a00-e1bb-46cc-94ba-3a0c2f8abdce)


* In the Create Hosted Zone pane, enter the name of the domain that you want to route traffic for, i'll use `julienmuke.cloud`. 
* For Type, accept the default value of Public Hosted Zone (if its VPC only then you can select private hosted Zone).


![Route-53-Global](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/597c2b2e-dbe7-4c6e-921c-a606e6600694)


* As you can see the NS and SOA record will be created.
* The next thing that you need to do is copy these name servers (Route traffic) and you need to add it in your Hosting domain 

![Screenshot 2024-04-18 at 15 30 31](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/1500e362-6f75-40bd-a616-c3277424f6f3)


* Navigate to your Cpanel, search for "Manage domain" or something similar to that, then change the "DNS/Nameservers" paste the name servers from AWS Route 53 Hosted Zones.

![Screenshot 2024-04-18 at 15 32 11](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/cd49d101-fbdf-418c-8dd0-0e1d569191df)


2. Edit CloudFront settings - by using custom URLs by adding alternate domain names.

When you create a distribution, CloudFront provides a domain name for it, such as d111111abcdef8.cloudfront.net. Instead of using this provided domain name, you can use an alternate domain name (also known as a CNAME).

Let see how to use your own domain name, such as www.example.com:

* Back to your CloudFront console, choose the ID for the distribution that you want to update.
* On the General tab, choose Edit.

![Screenshot 2024-04-18 at 15 01 01](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/3a8050b8-5081-4b88-9bbb-3b61b1dce445)

* Update the following values: `Alternate Domain Names (CNAMEs)`

Add your alternate domain names, i'll use a sub-domain `greeting.julienmuke.cloud` separate domain names with commas, or type each domain name on a new line.


![CloudFront-Global(2)(2)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/d3774de8-d1fe-4175-91cf-2fd5ccf6482e)


* Get an SSL/TLS certificate from an authorized certificate authority (CA) that covers the domain name. Add the certificate to your distribution to validate that you are authorized to use the domain.

![Screenshot 2024-04-18 at 15 36 10](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/ca12f4d2-4258-47d5-b48d-39a26c690e4b)


* Choose "Request a public certificate" request a public SSL/TLS certificate from Amazon. By default, public certicates are trusted by browsers and operating systems.

![Screenshot 2024-04-18 at 15 37 03](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/039483ec-0c9c-463c-8f14-19d319251463)


* Next you need to provide a fully qualified domain name `*.julienmuke.cloud`
* Leave everything else as default and click "Request"

![Certificate-Manager-us-east-1](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/9e0029a2-159d-4d49-b7a1-bc745e70c9f0)

NOTE: You have to wait a few minutes for the certificate to be issued.

* Next, to verify the SSL certificatethe, we need to select our new certificatethe we've created, complete the CloudFront settings and click "Save changes"

![CloudFront-Global(2)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/c49f28df-cc05-4239-bc49-2744d9fe42f8)



* Let create new record to validate the SSL/TLS certificate from Amazon.
* Go to your AWS Certificate Manager (ACM) and click on "Create record in route 53"

![Screenshot 2024-04-18 at 15 40 30](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/7db5024c-183e-44f0-97c3-068b1e6e9804)


* It is going to create a CNAME record in Route 53, then click on "Create record"


![Screenshot 2024-04-18 at 15 40 58](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/3a58fa42-42a3-48ab-81b5-6623a4753596)



* Next, let create another record for our subdomain `greeting.julienmuke.cloud` 
* Click on "Create record"
* The record name is `greeting` .julienmuke.cloud
* Enable "Alias"
* Choose route traffic: "Alias to CloudFront distribution"
* Choose the distribution listed, it will show the CloudFront with the SSL certificate that we configured.
* Click "Create records"


![Screenshot 2024-04-18 at 15 49 47](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/17c85177-d074-40c9-9de4-19bfd3668bb1)

What happens is that the routing will be made and the records will be created for `greeting.julienmuke.cloud` 


## ➡️ Step 4 - Test the Route 53 and the certificate for the CloudFront distribution

After you complete the CloudFront configuration, you will be able to access your website using `greeting.julienmuke.cloud` copy the URL and paste it on your web browser

![Screenshot 2024-04-18 at 15 50 52](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/8c1fbbc0-70d1-4814-bed8-b5938600c080)


* Below is the Live website with a simple functionality, you enter your full name, click "Submit" button and it will display you full name which is stored in the S3 Bucket.

![Screenshot 2024-04-18 at 16 07 15](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/ad4399e1-34e9-4558-aac3-770168fd50e0)

As you can see the website is working and the SSL Certificate is enabled.

## ➡️ Step 5 - Create a DynamoDB Table 

The next step is to create a DynamoDB and add views functionality on our webpage using a Lambda.

Feature: We have our static website, whenever someone is visiting the website the [ Views: ] counter will inscrease.

To create a DynamoDB Table:
1. Nagivate to your AWS Management Console and open the DynamoDB console
2. Choose Create table
3. Enter the Table details as follows:
<br/>* For Table name, enter `serverless-web-application-on-aws`
<br/>* For Partition key, enter `id`
4. For Table settings, keep the default selection of Default settings.
5. Choose Create table to create the table.

![Screenshot 2024-04-26 at 13 12 05](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/a67f9981-c9ae-437d-8661-dc6b85192b22)

6. Once the DynamoDB Table is created, click on the table name, and then click "Explore table items"

![Screenshot 2024-04-24 at 15 26 33](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/6b0a34b0-ffb0-4248-a5f3-c1c38fa459a8)

7. Choose "Create item"

![Screenshot 2024-04-24 at 15 27 34](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/7b8b1de5-d64d-45cc-9a3c-8e09fc5c9553)


8. You can add, remove, or edit attributes of an item:
<br>* For id partition key, enter `0`
<br>* Click "Add new attributes" and choose "Number"
<br>* For attribute name, enter `views`
<br>* For value, enter `1`

![Screenshot 2024-04-24 at 15 28 59](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/e579d2ac-1a54-4ca8-b52b-8737145142a6)


## ➡️ Step 6 - Create an IAM role

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources for your users. You use IAM to control who can use your AWS resources (authentication) and what resources they can use and in what ways (authorization).

To create an IAM role:

1. Go to the IAM Management Console, click Roles in the left nav pane, then click "Create role"

![Screenshot 2024-04-24 at 15 34 04](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/828892b3-3fd7-4df8-b148-36c37d365ef5)


2. Select type of trusted entity as `AWS service`
3. For servise or use case, select `lambda` then click "Next"

![Create-role-IAM-Global(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/7de73f84-81f4-4e09-9bb7-7186b8b7006f)


4. The Create role **Attach permissions policies** page opens with options for creating a new policy, refreshing the page, and searching existing policies named `AmazonDynamoDBFullAccess` and click "Next"


![Screenshot 2024-04-24 at 15 43 12](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/ab749afa-8680-4c11-8849-3b407e949e46)


5. For the role name, enter `serverless-web-application-on-aws`
6. Scroll and click "Create role"


![Create-role-IAM-Global(2)(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/1b00b2b6-6bc9-453c-a8fb-491fdb7257f2)


## ➡️ Step 7 - Create a Lambda function 

To create a Lambda function with the console:

1. Open the Functions page of the Lambda console
2. Choose Create function
3. Select Author from scratch
4. In the Basic information pane, for Function name enter `serverless-web-application-on-aws`
5. For Runtime, choose Python 3.8
6. Click on Advance settings, and select "Enable function URL" (This is to use function URLs to assign HTTP endpoints to your Lambda function).
7. For Auth type select "None"
You are allowing Lambda won't perform IIM authentication on request to your function URL basically the URL endpoint will be public.
8. Select Cross Origin Ratio Sharing (CORS) so that only the website that starts with `julienmuke.cloud` can access this particular Lambda function.
9. Leave architecture set to x86_64 and choose "Create function"

![Create-function-Lambda(1)](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/b2291fe1-27aa-44a9-873f-6d3ddbfafe23)


10. Modify the code in the console:

<br>*   Choose the Code tab: In the console's built-in code editor, you should see the function code that Lambda created. If you don't see the lambda_function.py tab in the code editor, select lambda_function.py in the file explorer as shown on the image below.
<br>*  Paste the following code into the lambda_function.py tab, replacing the code that Lambda created.
<br>*  Then click "Deploy"

```python
import json
import boto3
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('serverless-web-application-on-aws')
def lambda_handler(event, context):
    response = table.get_item(Key={
        'id':'0'
    })
    views = response['Item']['views']
    views = views + 1
    
    print(views)
    
    response = table.put_item(Item={
        'id':'0',
        'views': views
    })
    
    return views

```

![Screenshot 2024-04-24 at 16 12 13](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/402abe6d-1485-4242-adf9-5a5df54474a8)


11. Next, we've created an IAM role, now let's attache the IAM role to this Lambda function.
<br>*   Select the permission tab
<br>*   Click "Edit"

![Screenshot 2024-04-24 at 16 08 32](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/e5ffcc02-1df6-44c7-9ffa-436d97d378c9)


12. Srolll down to "Execution role" and select the lambda function we've created and click "Save"

![Edit-basic-settings-serverless-web-application-on-aws-Lambda](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/af46a797-eb56-4e98-946a-90655c6e6a1e)

13. Let test our lambda function:
<br>*   Click "Deploy" and then "Test"
<br>*   For configure test event, enter `TestEvent` and click "Save"
<br>*   The response will update to 2 and 

![Screenshot 2024-04-24 at 16 14 30](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/86ff7a1a-96c8-4c7a-b6d8-4d6965623ea3)

If you check our DynamoDB table and refesh the table, the value of views will update to 2 as well.

![Screenshot 2024-04-24 at 16 15 07](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/b3764ee9-0ace-4d3b-8c61-5188d490bd38)


* Next, we will edit the script.js file so that the views counter will display on the webpage, to do that:

<br>-  Go to your script.js file, open it in code editor of your choice
<br>-  Go to your Lambda function, copy your function URL

![Screenshot 2024-04-24 at 16 51 09](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/6a918fdc-a447-428e-abb1-54938f65f162)









