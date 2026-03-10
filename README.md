# project-using-lapm-S3-CloudFront-RDS

<h1>QUick Recap Steps</h1>

<h3>1. Launch Ec2 ,install nginx ,php ,php-fpm ,php-mysql connector , mariadb and start the services.<br>
   <br>
2. Create 'uploads' folder in html directory ,<br>
   assign '777' permission to upload the folder.<br>
   <br>
3. Create S3 bucket 'ACL enable' type .<br>
   <br>
4. Create RDS ,Create database name 'nameDB', <br>
   Create table name 'posts' .<br>
   <br>
5. Create CloudFront distribuction .<br>
   <br>
6. Install composer in html folder(directory) & install aws sdk in html folder using composer .<br></h3>

---
---
<br>
<br>
<br>

# 🚀 AWS Image Upload Project (EC2 + S3 + CloudFront + RDS)

This project demonstrates how to build a **cloud-based image upload application** using multiple AWS services.

The application allows users to:

* Upload images through a web form
* Store images in **Amazon S3**
* Deliver images via **CloudFront CDN**
* Store metadata in **Amazon RDS (MySQL)**
* Run the application on **EC2 with Nginx + PHP**

---

# 🏗️ Architecture

User → EC2 (Nginx + PHP) → S3 → CloudFront
                                                    ↘ RDS (MySQL)

---

# 1️⃣ Launch EC2 Instance

Launch an **EC2 instance** with the following configuration:

* **AMI:** Amazon Linux
* **Instance Type:** t2.micro
* **Security Group:**

  * SSH (22) → My IP
  * HTTP (80) → Anywhere

Connect to the server using SSH.

```
ssh -i server1.pem ec2-user@<EC2-Public-IP>
```

---

# 2️⃣ Install Web Server (Nginx + PHP)

Install **Nginx, PHP and MySQL client**.

```
sudo yum install nginx php php-fpm php-mysqlnd mariadb -y
```

Start and enable **Nginx**.

```
sudo systemctl start nginx
sudo systemctl enable nginx
```

Start and enable **PHP-FPM**.

```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

---

# 3️⃣ Setup Web Directory

Go to the web root directory.

```
cd /usr/share/nginx/html
```

Create the main **PHP test file**.

```
sudo nano index.php
```

Example content:

```
<?php
phpinfo();
?>
```

---

# 4️⃣ Create Upload Directory

Create a folder to temporarily store uploaded images.

```
sudo mkdir uploads
```

Give proper permissions to allow uploads.

```
sudo chmod -R 777 uploads
```

---

# 5️⃣ Connect to Amazon RDS

Connect to the **RDS MySQL database** from the EC2 instance.

```
mysql -h namedb.cl8mw8agi9uc.eu-north-1.rds.amazonaws.com -u admin -p
```

After login, create database and table if required.

Example table structure:

```
CREATE TABLE posts(
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(20),
url VARCHAR(200),
cfurl VARCHAR(200),
caption VARCHAR(200)
);
```

---

# 6️⃣ Install AWS SDK (Composer)

Install **Composer** and the **AWS PHP SDK**.

```
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

Install AWS SDK:

```
composer require aws/aws-sdk-php
```

This creates the **vendor/** directory used by the PHP application.

---

# 7️⃣ Create S3 Bucket

Create an **Amazon S3 bucket** to store uploaded images.

Example:

```
Bucket Name: lamp-image-upload-project
Region: eu-north-1
```

Images uploaded from EC2 will be stored here.

---

# 8️⃣ Create CloudFront Distribution

Create a **CloudFront distribution** and use the **S3 bucket as the origin**.

CloudFront provides a **global CDN URL** like:

```
https://dxxxxxxxx.cloudfront.net
```

This URL is used to deliver images faster.

---

# 9️⃣ Application Workflow

1. User opens the **upload form**
2. User selects an image and submits
3. Image is temporarily stored in **uploads/**
4. PHP uploads the image to **S3**
5. CloudFront URL is generated
6. Image metadata is stored in **RDS database**

---

# 🔟 Test the Application

Open the application in your browser:

```
http://<EC2-Public-IP>/posts.html
```

Upload an image and verify:

* Image stored in **S3**
* Image accessible via **CloudFront**
* Entry saved in **RDS database**

---

# ✅ AWS Services Used

* **Amazon EC2** – Application Server
* **Amazon S3** – Image Storage
* **Amazon CloudFront** – CDN Delivery
* **Amazon RDS** – MySQL Database

---

# 🎯 Outcome

This project demonstrates a **real-world cloud architecture** combining compute, storage, CDN, and database services on AWS.

