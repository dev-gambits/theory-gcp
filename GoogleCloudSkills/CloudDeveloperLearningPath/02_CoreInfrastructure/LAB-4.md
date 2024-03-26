# Hello Cloud Run

* [LAB-4](https://www.cloudskillsboost.google/course_templates/60/labs/460863)

### Hello Cloud Run
schedule
25 minutes
universal_currency_alt
5 Credits
Lab instructions and tasks
Overview
Setup and requirements
Task 1. Enable the Cloud Run API and configure your Shell environment
Task 2. Write the sample application
Task 3. Containerize your app and upload it to Artifact Registry
Task 4. Deploy to Cloud Run
Task 5. Clean up
End your lab
Congratulations!
Overview
Cloud Run Logo

Cloud Run is a managed compute platform that enables you to run stateless containers that are invocable via HTTP requests. Cloud Run is serverless: it abstracts away all infrastructure management, so you can focus on what matters most — building great applications.

Cloud Run is built from Knative, letting you choose to run your containers either fully managed with Cloud Run, or in your Google Kubernetes Engine cluster with Cloud Run on GKE.

The goal of this lab is for you to build a simple containerized application image and deploy it to Cloud Run.

Objectives
In this lab, you learn to:

Enable the Cloud Run API.
Create a simple Node.js application that can be deployed as a serverless, stateless container.
Containerize your application and upload to Container Registry (now called "Artifact Registry.")
Deploy a containerized application on Cloud Run.
Delete unneeded images to avoid incurring extra storage charges.
Setup and requirements
For each lab, you get a new Google Cloud project and set of resources for a fixed time at no cost.

Sign in to Qwiklabs using an incognito window.

Note the lab's access time (for example, 1:15:00), and make sure you can finish within that time.
There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click Start lab.

Note your lab credentials (Username and Password). You will use them to sign in to the Google Cloud Console.

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.
If you use other credentials, you'll receive errors or incur charges.

Accept the terms and skip the recovery resource page.

Note: Do not click End Lab unless you have finished the lab or want to restart it. This clears your work and removes the project.

How to start your lab and sign in to the Console
Click the Start Lab button. If you need to pay for the lab, a pop-up opens for you to select your payment method. On the left is a panel populated with the temporary credentials that you must use for this lab.

Credentials panel

Copy the username, and then click Open Google Console. The lab spins up resources, and then opens another tab that shows the Choose an account page.

Note: Open the tabs in separate windows, side-by-side.
On the Choose an account page, click Use Another Account. The Sign in page opens.

Choose an account dialog box with Use Another Account option highlighted 

Paste the username that you copied from the Connection Details panel. Then copy and paste the password.

Note: You must use the credentials from the Connection Details panel. Do not use your Google Cloud Skills Boost credentials. If you have your own Google Cloud account, do not use it for this lab (avoids incurring charges).
Click through the subsequent pages:
Accept the terms and conditions.
Do not add recovery options or two-factor authentication (because this is a temporary account).
Do not sign up for free trials.
After a few moments, the Cloud console opens in this tab.

Note: You can view the menu with a list of Google Cloud Products and Services by clicking the Navigation menu at the top-left. Cloud Console Menu
Activate Google Cloud Shell
Google Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud.

Google Cloud Shell provides command-line access to your Google Cloud resources.

In Cloud console, on the top right toolbar, click the Open Cloud Shell button.

Highlighted Cloud Shell icon

Click Continue.

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your PROJECT_ID. For example:

Project ID highlighted in the Cloud Shell Terminal

gcloud is the command-line tool for Google Cloud. It comes pre-installed on Cloud Shell and supports tab-completion.

You can list the active account name with this command:
gcloud auth list
Copied!
Output:

Credentialed accounts:
 - @.com (active)
Example output:

Credentialed accounts:
 - google1623327_student@qwiklabs.net
You can list the project ID with this command:
gcloud config list project
Copied!
Output:

[core]
project = 
Example output:

[core]
project = qwiklabs-gcp-44776a13dea667a6
Note: Full documentation of gcloud is available in the gcloud CLI overview guide .
Reference
Basic Linux Commands
Below you will find a reference list of a few very basic Linux commands which may be included in the instructions or code blocks for this lab.

Command -->	Action	.	Command -->	Action
mkdir (make directory)	create a new folder	.	cd (change directory)	change location to another folder
ls (list )	list files and folders in the directory	.	cat (concatenate)	read contents of a file without using an editor
apt-get update	update package manager library	.	ping	signal to test reachability of a host
mv (move )	moves a file	.	cp (copy)	makes a file copy
pwd (present working directory )	returns your current location	.	sudo (super user do)	gives higher administration privileges
Task 1. Enable the Cloud Run API and configure your Shell environment
From Cloud Shell, enable the Cloud Run API :
gcloud services enable run.googleapis.com
Copied!
If you are asked to authorize the use of your credentials, do so. You should then see a successful message similar to this one:
Operation "operations/acf.cc11852d-40af-47ad-9d59-477a12847c9e" finished successfully.
Note: You can also enable the API using the APIs & Services section of the console.
Set the compute region:
gcloud config set compute/region "REGION"
Copied!
Create a LOCATION environment variable:
LOCATION="Region"
Copied!
Task 2. Write the sample application
In this task, you will build a simple express-based NodeJS application which responds to HTTP requests.

In Cloud Shell create a new directory named helloworld, then move your view into that directory:
mkdir helloworld && cd helloworld
Copied!
Next you'll be creating and editing files. To edit files, use nano or the Cloud Shell Code Editor by clicking on the Open Editor button in Cloud Shell.

Create a package.json file, then add the following content to it:

nano package.json
Copied!
{
  "name": "helloworld",
  "description": "Simple hello world sample in Node",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "Google LLC",
  "license": "Apache-2.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}
Copied!
Most importantly, the file above contains a start script command and a dependency on the Express web application framework.

Press CTRL+X, then Y, then Enter to save the package.json file.

Next, in the same directory, create a index.js file, and copy the following lines into it:

nano index.js
Copied!
const express = require('express');
const app = express();
const port = process.env.PORT || 8080;

app.get('/', (req, res) => {
  const name = process.env.NAME || 'World';
  res.send(`Hello ${name}!`);
});

app.listen(port, () => {
  console.log(`helloworld: listening on port ${port}`);
});
Copied!

This code creates a basic web server that listens on the port defined by the PORT environment variable. Your app is now finished and ready to be containerized and uploaded to Container Registry.

Press CTRL+X, then Y, then Enter to save the index.js file.
Note: You can use many other languages to get started with Cloud Run. You can find instructions for Go, Python, Java, PHP, Ruby, Shell scripts, and others from the Quickstarts guide.
Task 3. Containerize your app and upload it to Artifact Registry
To containerize the sample app, create a new file named Dockerfile in the same directory as the source files, and add the following content:
nano Dockerfile
Copied!
# Use the official lightweight Node.js 12 image.
# https://hub.docker.com/_/node
FROM node:12-slim

# Create and change to the app directory.
WORKDIR /usr/src/app

# Copy application dependency manifests to the container image.
# A wildcard is used to ensure copying both package.json AND package-lock.json (when available).
# Copying this first prevents re-running npm install on every code change.
COPY package*.json ./

# Install production dependencies.
# If you add a package-lock.json, speed your build by switching to 'npm ci'.
# RUN npm ci --only=production
RUN npm install --only=production

# Copy local code to the container image.
COPY . ./

# Run the web service on container startup.
CMD [ "npm", "start" ]
Copied!
Press CTRL+X, then Y, then Enter to save the Dockerfile file.

Now, build your container image using Cloud Build by running the following command from the directory containing the Dockerfile. (Note the $GOOGLE_CLOUD_PROJECT environmental variable in the command, which contains your lab's Project ID):

gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld
Copied!
Cloud Build is a service that executes your builds on GCP. It executes a series of build steps, where each build step is run in a Docker container to produce your application container (or other artifacts) and push it to Cloud Registry, all in one command.

Once pushed to the registry, you will see a SUCCESS message containing the image name (gcr.io/[PROJECT-ID]/helloworld). The image is stored in Artifact Registry and can be re-used if desired.

List all the container images associated with your current project using this command:
gcloud container images list
Copied!
Register gcloud as the credential helper for all Google-supported Docker registries:
gcloud auth configure-docker
Copied!
Note: You may be prompted Do you want to continue? (y/N)? if you are, enter Y to agree.
To run and test the application locally from Cloud Shell, start it using this standard docker command:
docker run -d -p 8080:8080 gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld
Copied!
In the Cloud Shell window, click on Web preview and select Preview on port 8080.
This should open a browser window showing the "Hello World!" message. You could also simply use curl localhost:8080.

Task 4. Deploy to Cloud Run
Deploying your containerized application to Cloud Run is done using the following command adding your Project-ID:
gcloud run deploy --image gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld --allow-unauthenticated --region=$LOCATION
Copied!
The allow-unauthenticated flag in the command above makes your service publicly accessible.

When prompted confirm the service name by pressing Enter.
Note: You may be prompted Do you want enable these APIs to continue (this will take a few minutes)? (y/N)? if you are, enter Y to enable the API needed.
Wait a few moments until the deployment is complete.

On success, the command line displays the service URL:

Service [helloworld] revision [helloworld-00001-xit] has been deployed
and is serving 100 percent of traffic.

Service URL: https://helloworld-h6cp412q3a-uc.a.run.app
You can now visit your deployed container by opening the service URL in any browser window.

Congratulations! You have just deployed an application packaged in a container image to Cloud Run. Cloud Run automatically and horizontally scales your container image to handle the received requests, then scales down when demand decreases. In your own environment, you only pay for the CPU, memory, and networking consumed during request handling.

For this lab you used the gcloud command-line. Cloud Run is also available via Cloud Console.

From the Navigation menu, in the Serverless section, click Cloud Run and you should see your helloworld service listed:
Cloud Run tab displaying the helloworld service

Task 5. Clean up
While Cloud Run does not charge when the service is not in use, you might still be charged for storing the built container image.

You can either decide to delete your GCP project to avoid incurring charges, which will stop billing for all the resources used within that project, or simply delete your helloworld image using this command :
gcloud container images delete gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld
Copied!
When prompted to continue type Y, and press Enter.

To delete the Cloud Run service, use this command :

gcloud run services delete helloworld --region="REGION"
Copied!
When prompted to continue type Y, and press Enter.
End your lab
When you have completed your lab, click End Lab. Google Cloud Skills Boost removes the resources you’ve used and cleans the account for you.

You will be given an opportunity to rate the lab experience. Select the applicable number of stars, type a comment, and then click Submit.

The number of stars indicates the following:

1 star = Very dissatisfied
2 stars = Dissatisfied
3 stars = Neutral
4 stars = Satisfied
5 stars = Very satisfied
You can close the dialog box if you don't want to provide feedback.

For feedback, suggestions, or corrections, please use the Support tab.

Congratulations!
You have completed this lab!

Next steps / learn more
For more information on building a stateless HTTP container suitable for Cloud Run from code source and pushing it to Container Registry, view:

Developing Cloud Run services
Building Containers
Manual Last Updated December 12, 2023

Lab Last Tested December 12, 2023

Copyright 2022 Google LLC All rights reserved. Google and the Google logo are trademarks of Google LLC. All other company and product names may be trademarks of the respective companies with which they are associated.  