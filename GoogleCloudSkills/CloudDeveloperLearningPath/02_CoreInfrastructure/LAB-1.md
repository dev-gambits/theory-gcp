# Google Cloud Fundamentals: Getting Started with Cloud Marketplace

* [LAB-1](https://www.cloudskillsboost.google/course_templates/60/labs/460836)

Google Cloud Fundamentals: Getting Started with Cloud Marketplace
schedule
25 minutes
universal_currency_alt
5 Credits
Lab instructions and tasks
Overview
Objectives
Task 1. Sign in to the Google Cloud Console
Task 2. Use Cloud Marketplace to deploy a LAMP stack
Task 3. Verify your deployment
Congratulations!
End your lab
Overview
In this lab, you use Google Cloud Marketplace to quickly and easily deploy a LAMP stack on a Compute Engine instance. The Bitnami LAMP Stack provides a complete web development environment for Linux that can be launched in one click.

Component	Role
Linux	Operating system
Apache HTTP Server	Web server
MySQL	Relational database
PHP	Web application framework
phpMyAdmin	PHP administration tool
You can learn more about the Bitnami LAMP stack from the Bitnami Documentation article Google Cloud Platform.

Objectives
In this lab, you learn how to launch a solution using Cloud Marketplace.

Task 1. Sign in to the Google Cloud Console
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

Task 2. Use Cloud Marketplace to deploy a LAMP stack
In the Google Cloud Console, on the Navigation menu (Navigation menu icon), click Marketplace.

In the search bar, type LAMP and then press ENTER.

In the search results, click Bitnami package for LAMP.

If you choose another LAMP stack, such as the Google Click to Deploy offering, the lab instructions will not work as expected.

On the LAMP page, click GET STARTED.

On the Agreements page, check the box for Terms and agreements, and click AGREE.

On the Successfully agreed to terms pop up, click DEPLOY.

If this is your first time using Compute Engine, the Compute Engine API must be initialized before you can continue.

For Zone, select the deployment zone that Qwiklabs assigned to you.

For Machine Type, select E2 as the Series and e2-medium as the Machine Type.

Leave the remaining settings as their defaults.

Click Deploy.

If a Welcome to Deployment Manager message appears, click Close to dismiss it.

The status of the deployment appears in the console window: lampstack-1 is being deployed. When the deployment of the infrastructure is complete, the status changes to lampstack-1 has been deployed.

After the software is installed, a summary of the details for the instance, including the site address, is displayed.

Click Check my progress to verify the objective.
Use Cloud Marketplace to deploy a LAMP stack

Task 3. Verify your deployment
When the deployment is complete, click the Site address link in the right pane. (If the website is not responding, wait 30 seconds and try again.) If you see a redirection notice, click on that link to view your new site.

Alternatively, you can click Visit the site in the Get started with Bitnami package for LAMP section of the page. A new browser tab displays a congratulations message. This page confirms that, as part of the LAMP stack, the Apache HTTP Server is running.

Close the congratulations browser tab.

On the Google Cloud Console, under Get started with LAMP Packaged by Bitnami, click SSH.

In a new window, a secure login shell session on your virtual machine appears.

In the SSH window, to change the current working directory to /opt/bitnami, execute the following command:

cd /opt/bitnami
Copied!
To copy the phpinfo.php script from the installation directory to a publicly accessible location under the web server document root, execute the following command:

sudo sh -c 'echo "<?php phpinfo(); ?>" > apache2/htdocs/phpinfo.php'
Copied!
The phpinfo.php script displays your PHP configuration. It is often used to verify a new PHP installation.

To close the SSH window, execute the following command:

exit
Copied!
Open a new browser tab.

Enter the following URL, and replace SITE_ADDRESS with the URL in the Site address field in the right pane of the lampstack page.

http://SITE_ADDRESS/phpinfo.php
Copied!
A summary of the PHP configuration of your server is displayed.

Note: If you can't see the web page in the browser on your corporate laptop: If possible, exit any corporate VPN/network and try again. Enter the IP address on another device, such as a tablet or even a phone.
Close the phpinfo tab.
Congratulations!
In this lab, you deployed a LAMP stack to a Compute Engine instance.

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

Copyright 2022 Google LLC All rights reserved. Google and the Google logo are trademarks of Google LLC. All other company and product names may be trademarks of the respective companies with which they are associated.

More resources
Read the Google Cloud documentation on Cloud Marketplace.


