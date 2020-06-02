# GitHub Code Repository

## Introduction

For this workshop we will use GitHub as a central code repository and version control system.

Navigate to [https://github.com](https://github.com/) and create an account.

Fork [vltabacaru/orcl-ws-cicd](https://github.com/vltabacaru/orcl-ws-cicd) project in your account. It creates a copy of this repository in your account [Your Username]/orcl-ws-cicd. In your repository, click on **Clone or download**, and copy the URL in your notes text file. It looks like https://github.com/[Your Username]/orcl-ws-cicd.git.

>**Important Note** : All exercises for this workshop have to be executed on **sandbox_vm** as **userXX** user assigned by instructor.

As a file editor, you have two options, use the command line and **vim** editor, or use the Remote Desktop connection and **gedit** editor. 

1. If you know vim, please change all *gedit* commands with *vi*, and login as oracle user in the command line: 

2. If you use the Secure Global Desktop remote access, right-click and open a Terminal window, and maximize it.

Config Git to save credentials for 8 hours.

````
git config --global credential.helper 'cache --timeout 28800'
````

## Step 1: Start Application Development

Make a local clone on the development machine of the GitHub repository you forked from **vltabacaru/orcl-ws-cicd**.

````
git clone https://github.com/[Your Username]/orcl-ws-cicd.git
````

During all these labs, make sure you are in **~/orcl-ws-cicd** folder.

````
cd orcl-ws-cicd

chmod 600 keys/*
````
This repository contains only three files, a readme, and two template files we will use for the deployment of our web service application. It also contains a folder with sample SSH keys to be used as example only.

````
ls
keys  kubernetes_deployment.yml.template  kubernetes_service.yml.template  README.md
````

Edit both templates using your favourite editor, and change application name **orcl-ws-app**, container name **orcl-ws-srv**, and port number **8080** with your personal values: **usrXX-ws-app**, **usrXX-ws-srv**, and **80XX**, where XX is your **userXX** number. You can use the following commands to apply these changes.

````
sed -i -e 's/orcl-ws-app/usrXX-ws-app/g' kubernetes_deployment.yml.template

sed -i -e 's/orcl-ws-srv/usrXX-ws-srv/g' kubernetes_deployment.yml.template

sed -i -e 's/8080/80XX/g' kubernetes_deployment.yml.template

sed -i -e 's/orcl-ws-app/usrXX-ws-app/g' kubernetes_service.yml.template

sed -i -e 's/8080/80XX/g' kubernetes_service.yml.template
````

## Step 2: Start Application Development

For the entire development activity on this environment we will use a Python virtual environment. Create one called **orclvenv**.

````
python3 -m venv orclvenv
````

Activate Python virtual environment orclvenv.

````
. orclvenv/bin/activate
````

Create a new file called **promotion.py** 

````
gedit promotion.py
````

Write the following code:

````
"""
Simple Python application to show CI/CD capabilities.
"""

def addition(salary, amount):
    return salary + amount

def increment(salary, percentage):
    return salary * (1 + percentage/100)
````

Commit changes to central repository.

````
git add promotion.py
git commit -m "First two functions to calculate salary"
git push
````

If all is good, you should see promotion.py file in your forked repository at https://github.com/[Your Username]/orcl-ws-cicd. Refresh the page, and click on the file to see the contents.

## Step 3: Implement Automated Testing

For automated testing, we will use three Python tools:

1. flake8 (called linter) - used to check if the code conforms to the Python coding standards, and analyze code for potential errors;
2. pytest - a standard Python unit testing library designed to check a single unit of code (in our case a function);
3. pytest-cov - extension of pytest, that calculates the percentage of source code that is covered by unit tests.

It is a good practice to update pip installation tool.

````
pip install --upgrade pip
````

We install these tools in our Python virtual environment orclvenv.

````
pip install flake8 pytest pytest-cov
````

These packages will be required by other developers, so we need to share them on the central repository. Capture installed packages in a file requirements.pip.

````
pip freeze > requirements.pip
git add requirements.pip
git commit -a -m "Add requirements list"
git push
````

Run flake8 to analyze your code.

````
flake8 --exclude=orclvenv* --statistics
./promotion.py:5:1: E302 expected 2 blank lines, found 1
./promotion.py:8:1: E302 expected 2 blank lines, found 1
./promotion.py:10:1: W391 blank line at end of file
2     E302 expected 2 blank lines, found 1
1     W391 blank line at end of file
````

The response gives a list of issues that do not comply with Python programming standards, specifying the file name and line number of the occurrence. Python standards require two blank lines before a function definition, and no blank lines at the end of the file. Go ahead, fix the code, and test it again.

````
git commit -a -m "Automated test results fixed"
git push
````

## Step 4: Write Automated Unit Testing

Let's write unit tests for our code. Create a new file **test_promotion.py** with the following code:

````
"""
Unit tests for simple Python application
"""

import promotion


class TestPromotion:

    def test_addition(self):
        assert 1200 == promotion.addition(1150, 50)

    def test_increment(self):
        assert 1250 == promotion.increment(1000, 25)
````

Run pytest-cov to verify how much of your code is covered by unit tests, by using --cov flag with pytest. Pytest knows our file with the prefix test contains unit tests for our application. Same rules apply for class and method names inside the file.

````
pytest -v --cov=promotion

... 
collected 2 items  
 
test_promotion.py::TestPromotion::test_addition PASSED                [ 50%]
test_promotion.py::TestPromotion::test_increment PASSED               [100%]
...
Name           Stmts   Miss  Cover
----------------------------------
promotion.py       4      0   100%
````

In the response we can see our code is covered 100% by unit tests, this is the ideal development situation. Now we can commit the unit tests and push all code to the master branch.

````
git add test_promotion.py
git commit -a -m "Add automatic testing"
git push
````

## Acknowledgements

- **Author** - Valentin Leonard Tabacaru
- **Last Updated By/Date** - Valentin Leonard Tabacaru, Principal Product Manager, DB Product Management, May 2020

See an issue? Please open up a request [here](https://github.com/oracle/learning-library/issues). Please include the workshop name and lab in your request.

