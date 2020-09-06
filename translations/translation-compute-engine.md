## Lab Google Cloud Fundamentals: Getting Started with Compute Engine 

In this lab, you will learn how to perform the following tasks:

    - Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

    - Create a Compute Engine virtual machine using the gcloud command-line interface.

    - Connect between the two instances.

## Steps to Follow:

1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

    gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=e2-medium--image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-type=pd-standard --tags=http-server 

2. Create a Compute Engine virtual machine using the gcloud command-line interface.

    gcloud config set compute/zone us-central1-b

    gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

3. Connect between the two instances.

    a) Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
     
        i) Connect to my-vm-2 via ssh

            gcloud compute ssh my-vm-2

        ii) Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network then ctrl + c to stop the pinging

            ping my-vm-1

            clear

        iii) Use the ssh command to open a command prompt on my-vm-1 from my-vm-2

            ssh my-vm-1

        iv) At the command prompt on my-vm-1, install the Nginx web server

            sudo apt-get install nginx-light -y

        v) Use the nano text editor to add a custom message to the home page of the web server:

            sudo nano /var/www/html/index.nginx-debian.html

        vi) Add text like this, and replace YOUR_NAME with your name:

            Hi from Samuel Mulwa

        vii) Confirm that the web server is serving your new page. At the command prompt on my-vm-1

            curl http://localhost/

        viii) Exit the command prompt on my-vm-1 using the command below

            exit

        ix) Confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2

            curl http://my-vm-1/
        
        x) Get the external IP Address of my-vm-1 using the command

            gloud compute instances list

        xi) Copy and paste the external ip address and paste it into the web browser tab. For example

            http://35.184.107.178