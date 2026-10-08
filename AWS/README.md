
## Experiment 3: Launching an Amazon EC2 Instance

**Aim:** Launch an Ubuntu virtual server on AWS and confirm it is running.

### Steps
1. Sign in to the AWS Management Console.
2. Search for **EC2** and open the EC2 dashboard.
3. Choose a Region (e.g., Asia Pacific (Mumbai) or US East (N. Virginia)).
4. Click **Launch instance**.
5. Under **Name and tags**, give the instance a name (e.g., `My-Ubuntu-Server`).
6. Under **Application and OS Images (AMI)**, choose **Ubuntu** and make sure **Free tier eligible** is shown.
7. Under **Instance type**, choose `t2.micro` (or `t3.micro`).
8. Under **Key pair**, click **Create new key pair**, select **RSA** and **.pem**, then download the key file.
9. Under **Network settings**, allow **SSH** traffic from **My IP**. Optionally allow **HTTP** traffic.
10. Leave storage at the defaults and click **Launch instance**.
11. Click **View all instances** and check that the state changes from *Pending* to *Running*.

**Result:** The Ubuntu instance is launched and in the *Running* state.

---

## Experiment 8: Deploying a Web Application on Amazon EC2

**Aim:** Host a simple web page on an EC2 instance using the Apache HTTP server.

### Steps
1. Open the EC2 dashboard and click **Launch instance**.
2. Name the instance `web-server`.
3. Choose **Amazon Linux** as the AMI.
4. Choose `t3.micro` as the instance type.
5. Create a new key pair in **.pem** format and download it.
6. Under **Network settings**, allow **SSH**, **HTTP** and **HTTPS** traffic.
7. Keep the default storage and launch the instance.
8. Go to **Instances**, select `web-server`, click **Connect**, and use **EC2 Instance Connect**.
9. Switch to the root user:
   ```bash
   sudo su -
   ```
10. Update the system packages:
    ```bash
    yum update -y
    ```
11. Install the Apache HTTP server:
    ```bash
    yum install -y httpd
    ```
12. Move to the web root directory:
    ```bash
    cd /var/www/html
    ```
13. Create the index file:
    ```bash
    nano index.html
    ```
14. Paste the following HTML:
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>My EC2 Web Application</title>
    </head>
    <body>
        <h1>Hello from AWS EC2!</h1>
        <p>This web page is deployed on an EC2 instance.</p>
        <p>Cloud Computing Lab</p>
    </body>
    </html>
    ```
15. Save and exit nano (`Ctrl + O`, `Enter`, `Ctrl + X`).
16. Enable Apache to start on boot and start it now:
    ```bash
    systemctl enable httpd
    systemctl start httpd
    ```
17. Check that the service is running:
    ```bash
    systemctl status httpd
    ```
18. Copy the instance's **Public IPv4 address** and open it in a browser to view the page.

**Result:** The web page is served from the EC2 instance's public IP.

---

## Experiment 10: Hosting a Static Website on Amazon S3

**Aim:** Publish a static website using S3 static website hosting.

### Steps
1. Open the **S3** dashboard and click **Create bucket**.
2. Enter a globally unique bucket name and choose a Region.
3. Uncheck **Block all public access** and acknowledge the warning.
4. Leave the other settings at their defaults and click **Create bucket**.
5. Open the bucket, go to the **Properties** tab and scroll to **Static website hosting**.
6. Click **Edit**, choose **Enable**, set the hosting type to **Host a static website**, enter `index.html` as the index document, and save.
7. Note the **Bucket website endpoint** URL.
8. Go to the **Permissions** tab, click **Edit** under **Bucket policy**, and paste the following read policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
       }
     ]
   }
   ```
9. Replace `YOUR_BUCKET_NAME` with your actual bucket name and save.
10. Create `index.html` on your computer with this content:
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>My AWS Static Website</title>
    </head>
    <body>
        <h1>Welcome to My AWS Website</h1>
        <p>This website is hosted using Amazon S3.</p>
        <p>Static Web Application</p>
    </body>
    </html>
    ```
11. In the bucket's **Objects** tab, click **Upload**, add `index.html`, and confirm.
12. Open the bucket website endpoint in a browser to verify the site.

**Result:** The static website is publicly reachable at the S3 website endpoint.

---

## Experiment 11: Content Delivery and Media Streaming with Amazon CloudFront

**Aim:** Serve a video file from a private S3 bucket through a CloudFront distribution.

### Steps
1. Open the **S3** dashboard and click **Create bucket**.
2. Enter a unique bucket name and choose a Region.
3. Keep **Block all public access** turned on and leave **SSE-S3** encryption enabled.
4. Click **Create bucket**.
5. Open the bucket, click **Upload**, select an `.mp4` file, and upload it.
6. Search for **CloudFront** in the console and click **Create distribution**.
7. Under **Origin domain**, select the S3 bucket you created.
8. Under **Origin access**, choose **Origin access control settings (OAC)** and create a control setting with the default values.
9. Under **Default cache behavior**, set **Viewer protocol policy** to **Redirect HTTP to HTTPS**.
10. Set **Allowed HTTP methods** to **GET, HEAD** and **Cache policy** to **CachingOptimized**.
11. Under **Web Application Firewall (WAF)**, choose **Do not enable security protections**.
12. Click **Create distribution** and wait for deployment to complete.
13. Copy the policy that CloudFront generates and paste it into the S3 bucket policy so OAC can access the bucket.
14. Copy the **Distribution domain name**, append your file name (e.g., `https://DISTRIBUTION_DOMAIN/video.mp4`), and open it in a browser to test streaming.

**Result:** The video streams through CloudFront while the S3 bucket stays private.