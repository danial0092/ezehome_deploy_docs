# Eze-Home Deployment Guide

Inspired By digitalocean docs [How To Set Up Django with Postgres, Nginx, and Gunicorn on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu#step-6-testing-gunicorn-s-ability-to-serve-the-project).

## Introduction
This document provides a structured step-by-step deployment guide for Django applications set up specifically on Ubuntu Server Latest Version.

## Prerequisites
*  - Initial Server Setup with Ubuntu.
*  - Creating a New User Dedicated to EzeHome.

### Initial Server Setup with Ubuntu
1. Logging in as root
2. Creating a New User
3. Granting Administrative Privileges


## Step 1 — Logging in as root
To log into your server, you will need to know your server’s public IP address. You will also need the password or the private key for the root user’s account if you installed an SSH key for authentication. If you have not already logged into your server.

If you are not connected to your server currently, log in as the root user using the following command. Substitute the highlighted your_server_ip portion of the command with your server’s
## Step 2 — Creating a New User
Once you log in as root, you’ll be able to add the new user account. In the future, we’ll log in with this new account instead of root.

This example creates a new user called sammy, but you should replace that with a username that you like:

``` 
# adduser sammy 

```

You will be asked a few questions, starting with the account password.

Enter a strong password and, optionally, fill in any additional information you would like. This information is not required, and you can press ENTER in any field you wish to skip.

## Step 3 — Granting Administrative Privileges

Now you have a new user account with regular account privileges. However, you will sometimes need to perform administrative tasks as the root user.

To avoid logging out of your regular user and logging back in as the root account, you can set up what is known as superuser or root privileges for your user’s regular account. These privileges will allow your normal user to run commands with administrative privileges by putting the word sudo before the command.

To add these privileges to your new user, you will need to add the user to the sudo system group. By default on Ubuntu, users who are members of the sudo group are allowed to use the sudo command.

As root, run this command to add your new user to the sudo group (substitute the highlighted `sammy` username with your new user):

``` 
# usermod -aG sudo sammy 

```

### Steps to Setup Django, Nginx & Gunicorn

## Step 1 — Installing the Packages from the Ubuntu Repositories

To begin the process, you will download and install all of the items that you need from the Ubuntu repositories. Later you will use the Python package manager pip to install additional components.

First you need to update the local apt package index and then download and install the packages. The packages that you install depend on which version of Python your project will use.

If you are using Django with Python 3, type:

``` 
sudo apt update
sudo apt install python3-venv python3-dev libpq-dev postgresql postgresql-contrib nginx curl
```
This command will install a tool to create virtual environments for your Python projects, the Python development files needed to build Gunicorn later, the Postgres database system and the libraries needed to interact with it, and the Nginx web server.

## Step 2 — Creating the PostgreSQL Database and User

Now you can jump right in and create a database and database user for our Django application.

By default, Postgres uses an authentication scheme called “peer authentication” for local connections. Basically, this means that if the user’s operating system username matches a valid Postgres username, that user can login with no further authentication.

During the Postgres installation, an operating system user named postgres was created to correspond to the ==postgres== PostgreSQL administrative user. You need to use this user to perform administrative tasks. You can use sudo and pass in the username with the -u option.

Log into an interactive Postgres session by typing:

``` 

$ sudo -u postgres psql

```

You will be given a PostgreSQL prompt where you can set up our requirements.

First, create a database for your project:

```
postgres=# CREATE DATABASE myproject;
``` 