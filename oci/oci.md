# Oracle Cloud Infrastructure (OCI)

## Introduction

Development and deployment platforms can be created on Oracle Cloud Infrastructure. Development platform is a Compute Node using Cloud Developer Image, having some development packages and tools pre-installed. Deployment is performed on a Docker Container running in Container Cluster (OKE). At the same time, Oracle Cluster Infrastructure Registry (OCIR) is used to store the Docker Image resulted from the automated build, and used for the deployment. Application data is store inside Oracle Database Cloud Service, running on a Database System, and using Multitenant Architecture for consolidation and resource optimization.

In this lab we will create all the required components on OCI.

## Step 1: Retrieve the SSH Key on Your Laptop

Open the web browser on your laptop and navigate to Oracle Cloud console. 

- URL :	https://console.eu-frankfurt-1.oraclecloud.com/
- Cloud tenant : provided by instructor
- Username : provided by instructor
- Password : provided by instructor

Click on hamburger menu ≡, then Object Storage > Object Storage.

Select the Compartment provided by instructor in the lower left drop-down list.

Click Lab-Artefacts bucket. Click the ⋮ menu next to **id_rsa** key file for Mac/Linux or **id_rsa.ppk** key file for Windows, and Download. Save file. This is the SSH private key.

## Step 2: Connect with Secure Shell

Use the SSH private key to connect to the sandbox machine. At the same time, add two tunnels for ports 8080 and 8001 we will need later in this workshop.

````
ssh -C -i /path/to/id_rsa -L 8080:localhost:8080 -L 8001:localhost:8001 opc@[sandbox_vm]
````

## Step 3: Connect with Oracle Secure Global Desktop

Open your browser and navigate to the SGD URL provided by instructor. Use the username and password provided by instructor.

This is the SDG sandbox machine you can use to launch any tool that requires a graphical interface, similar to a remote desktop connection.

Right click on the SGD desktop and Open Terminal. Maximize the terminal window.

## Step 4: Retrieve SSH Keys on SGD Sandbox

Click on Activities > Firefox. Navigate to Oracle Cloud console. 

- URL :	https://console.eu-frankfurt-1.oraclecloud.com/
- Cloud tenant : provided by instructor
- Username : provided by instructor
- Password : provided by instructor

Click on hamburger menu ≡, then Object Storage > Object Storage.

Select the Compartment provided by instructor in the lower left drop-down list.

Click Lab-Artefacts bucket. Click the ⋮ menu next to each one of the three id_rsa key files, and Download. Save file.

Click on Activities and select Terminal window. Check the downloaded files.

````
ls -lah ~/Downloads/
total 16K
drwxr-xr-x.  2 user01 user01   60 May 13 12:56 .
drwx------. 17 user01 user01 4.0K May 13 09:18 ..
-rw-rw-r--.  1 user01 user01 2.4K May 13 12:55 id_rsa
-rw-rw-r--.  1 user01 user01  590 May 13 12:55 id_rsa.pub
-rw-rw-r--.  1 user01 user01  625 May 13 12:56 id_rsa_pub.pem
````

Set the right permissions for all key files.

````
chmod 600 Downloads/id_rsa*
````

>**Note** : If you don't have three key files, starting with id_rsa, repeat the previous step.

## Step 5: Gather OCID Values

During the workshop, we will need some OCID values from Oracle Cloud console. Get these values and save them in your text notes file.

### User OCID

On Oracle Cloud console, or click on profile icon 👤 on upper right corner, then on the name of your user. Copy OCID: ocid1.user.oc1..aa[some_long_string]xi5q

### Tenancy OCID

Click on hamburger menu ≡, then Administration > **Tenancy Details**. Copy OCID: ocid1.tenancy.oc1..aa[some_long_string]3gfa

### Compartment OCID

Click on hamburger menu ≡, then Identity > **Compartments**. Click on your Compartment. Copy OCID: ocid1.compartment.oc1..aa[some_long_string]s6ha

## Acknowledgements

- **Author** - Valentin Leonard Tabacaru
- **Last Updated By/Date** - Valentin Leonard Tabacaru, Principal Product Manager, DB Product Management, May 2020

See an issue? Please open up a request [here](https://github.com/oracle/learning-library/issues). Please include the workshop name and lab in your request.

