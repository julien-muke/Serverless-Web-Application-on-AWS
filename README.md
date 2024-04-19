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

A hosted Zone one tells Route 53 how to respond to DNS queries for a domain such as example.com or julienmuke.cloud

