## Lab Google Cloud Platform Fundamentals - Core Infrastructure

In this lab, you create and deploy a simple App Engine application using a virtual environment in the Google Cloud Shell.

    - Initialize App Engine.

    - Preview an App Engine application running locally in Cloud Shell.

    - Deploy an App Engine application, so that others can reach it.

    - Disabl`e an App Engine application, when you no longer want it to be visible.

## Steps to Follow:
    - Open Cloud Shell

    - You can list the active account name with this command

        gcloud auth list
        
    - You can list the project ID with this command

        gcloud config list project

1.Initialize App Engine.

    a) Initialize your App Engine app with your project and View its region:

        gcloud app create --project=$DEVSHELL_PROJECT_ID

        Add a numeric number to select where you want your App Engine application located For example  [1] asia-east2 

        View its region by running the following command 
            
            gcloud app describe
    
    b) Clone the source code repository for a sample application in the hello_world directory

        git clone https://github.com/GoogleCloudPlatform/python-docs-samples

    c) Navigate to the source directory

        cd python-docs-samples/appengine/standard_python3/hello_world


2.Preview an App Engine application running locally in Cloud Shell.

    a) While in a command prompt cloud shell execute the following command to download and update the packages list

        sudo apt-get update

    b) Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system.

        sudo apt-get install virtualenv

        virtualenv -p python3 venv

    c) Activate the virtual environment.

        source venv/bin/activate

    d) Navigate to your project directory and install dependencies.

        pip install  -r requirements.txt

    e) Run the application

        python main.py

3.Deploy an App Engine application, so that others can reach it.

    a) Navigate to the source directory in the cloud shell:

        cd ~/python-docs-samples/appengine/standard_python3/hello_world

    b) Deploy your Hello World application.

        gcloud app deploy

    c)Launch your browser to view the app at http://YOUR_PROJECT_ID.appspot.com

        gcloud app browse


4.Disable an App Engine application, when you no longer want it to be visible.

    Disable the application by using the App ID


