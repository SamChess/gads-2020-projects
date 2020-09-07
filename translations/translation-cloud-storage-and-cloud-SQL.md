## Lab Getting Started with Cloud Storage and Cloud SQL

In this lab, you learn how to perform the following tasks:

    - Create a Cloud Storage bucket and place an image into it.

    - Create a Cloud SQL instance and configure it.

    - Connect to the Cloud SQL instance from a web server.

    - Use the image in the Cloud Storage bucket on a web page.

## Steps to Follow:

1.Deploy a web server VM instance named bloghost

    gcloud compute instances create bloghost --zone=us-central1-a --machine-type=e2-medium --subnet=default --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --tags=http-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=bloghost

2.Create a Cloud Storage bucket using the gsutil command line

    a) Activate Cloud Shell 

    b) Enter your chosen location into an environment variable called LOCATION

        export LOCATION=US

    c) The DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID:

        gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

    d) Retrieve a banner image from a publicly accessible Cloud Storage location:

        gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

    e) Copy the banner image to your newly created Cloud Storage bucket:

        gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

    f) Modify the Access Control List of the object you just created so that it is readable by everyone:

        gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

3.Create the Cloud SQL instance

    gcloud sql instances create blog-db --password="Street writer 1997" --sql-version=MYSQL_5_7" --zone=us-central1-a

    gcloud sql users create blogdbuser --instance=blog-db -i blog-db --host='%' --password='The grand tour 2020'

4.Configure an application in a Compute Engine instance to use Cloud SQL

    a) In the VM instances list, click SSH in the row for your VM instance bloghost.

         gcloud compute ssh bloghost

    b) Change the working directory to the document root of the web server:

        cd /var/www/html

    c) Edit a file called index.php using the nano text editor and Paste the content below into the file:

        <html>
            <head>
                <title>Welcome to my excellent blog</title>
            </head>
            <body>
                <h1>Welcome to my excellent blog</h1>
                <?php
                $dbserver = "CLOUDSQLIP";
                $dbuser = "blogdbuser";
                $dbpassword = "DBPASSWORD";
                // In a production blog, we would not store the MySQL
                // password in the document root. Instead, we would store it in a
                // configuration file elsewhere on the web server VM instance.

                $conn = new mysqli($dbserver, $dbuser, $dbpassword);

                if (mysqli_connect_error()) {
                        echo ("Database connection failed: " . mysqli_connect_error());
                } else {
                        echo ("Database connection succeeded.");
                }
                ?>
            </body>
        </html>

    d) Restart the web server using the command below:

        sudo service apache2 restart

    e) Open a new web browser tab and paste into the address bar your bloghost VM instance's external IP address followed by /index.php.

        For example, 35.172.198.4/index.php

        When you load this you should see the error : Database connection failed: ...

    f) Return to your ssh session on bloghost. Use the nano text editor to edit index.php again using the command.

        sudo nano index.php

    g) In the nano text editor, replace CLOUDSQLIP with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.

    For example,  $dbserver = "CLOUDSQLIP" to $dbserver = "34.192.178.22"

    h) In the nano text editor, replace DBPASSWORD with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place and save the changes.

    For example,  $dbpassword = "DBPASSWORD"; to $dbpassword = "The Grand Tour 2020";

    i) Restart the web server:

        sudo service apache2 restart

    j) Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, the following message appears:

        Database connection succeeded.

5.Configure an application in a Compute Engine instance to use a Cloud Storage object

    a) Click on the bucket that is named after your GCP project. In this bucket, there is an object called my-excellent-blog.png. Copy the URL behind the link icon that appears in that object's Public access column, or behind the words "Public link" if shown.

    b) ssh session on your bloghost VM instance

        gcloud compute ssh bloghost

    c) Enter this command to set your working directory to the document root of the web server:

        cd /var/www/html

    d) Use the nano text editor to edit index.php:

        sudo nano index.php

    e) Paste this HTML markup after the h1 element and save. For example, 

        <img src='https://storage.googleapis.com/qwiklabs-gcp-0005e186fa559a09/my-excellent-blog.png'>

    f) Restart the web server:

        sudo service apache2 restart

    g) Return to the web browser tab in which you opened your bloghost VM instance's external IP address. When you load the page, its content now includes a banner image.




