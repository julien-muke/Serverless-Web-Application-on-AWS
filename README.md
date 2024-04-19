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


➡️ Step 1 - Create an Amazon S3 Bucket

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

4. Choose a file to upload, we are going to upload the code provided in this GitHub repo, file name: `index.html` `script.j` `style.css`, and then choose Open, and choose Upload.

![Screenshot 2024-04-12 at 11 34 37](https://github.com/julien-muke/Serverless-Web-Application-on-AWS/assets/110755734/56a008f4-18a3-452d-8af2-d08d66ba40c0)


You've successfully uploaded an object to your bucket. 



