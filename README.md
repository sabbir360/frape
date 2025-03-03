# Bench

[![Build Status](https://travis-ci.org/frappe/bench.svg?branch=master)](https://travis-ci.org/frappe/bench)

The bench is a command-line utility that helps you to install apps, manage multiple sites and update Frappe / ERPNext apps on */nix (CentOS 6, Debian 7, Ubuntu, etc) for development and production. Bench will also create nginx and supervisor config files, setup backups and much more.

If you are using on a VPS make sure it has >= 1Gb of RAM or has swap setup properly.

To do this install, you must have basic information on how Linux works and should be able to use the command-line. If you are looking easier ways to get started and evaluate ERPNext, [download the Virtual Machine](https://erpnext.com/download) or take [a free trial on erpnext.com](https://erpnext.com/pricing).

If you have questions, please ask them on the [forum](https://discuss.erpnext.com/).

## Installation

## Manual Install

To manually install frappe/erpnext here are the steps

#### 1. Install Pre-requisites

- Python 2.7
- MariaDB 10+
- Nginx (for production)
- Nodejs
- Redis
- wkhtmltopdf with patched Qt (for pdf generation)

#### 2. Install Bench

Install bench as a *non root* user,

	git clone https://github.com/sabbir360/bench bench-repo
	sudo pip install -e bench-repo

Note: Please do not remove the bench directory the above commands will create

#### Basic Usage

* Create a new bench

	The init command will create a bench directory with frappe framework
	installed. It will be setup for periodic backups and auto updates once
	a day.

		bench init frappe-bench && cd frappe-bench

* Add apps

	The get-app command gets remote frappe apps from a remote git repository and installs it. Example: [erpnext](https://github.com/sabbir360/erpnext)

		bench get-app erpnext https://github.com/sabbir360/erpnext

* Add site

	Frappe apps are run by frappe sites and you will have to create at least one
	site. The new-site command allows you to do that.

		bench new-site site1.local

* Install erpnext

	To install erpnext on your new site, use the bench `install-app` command

		bench --site site1.local install-app erpnext

* Start bench

	To start using the bench, use the `bench start` command

		bench start

	To login to Frappe / ERPNext, open your browser and go to `localhost:8000`

	The default user name is "Administrator" and password is what you set when you created the new site.


---

## Easy Install

- This is an opinionated setup so it is best to setup on a blank server.
- Works on Ubuntu 14.04 to 16.04, CentOS 7+, Debian 7 to 8 and MacOS X.
- You may have to install Python 2.7 (eg on Ubuntu 16.04+) by running `apt-get install python-minimal`
- This script will install the pre-requisites, install bench and setup an ERPNext site
- Passwords for Frappe Administrator and MariaDB (root) will be asked
- You can then login as **Administrator** with the Administrator password
- If you find any problems, post them on the forum: [https://discuss.erpnext.com](https://discuss.erpnext.com)

Open your Terminal and enter:

#### 1. Download the install script

For Linux:

	wget https://raw.githubusercontent.com/sabbir360/bench/master/playbooks/install.py

For Mac OSX:

Install X Code (from App store) and HomeBrew (http://brew.sh/) first

	brew install python
	brew install git
	curl "https://raw.githubusercontent.com/sabbir360/bench/master/playbooks/install.py" -o install.py

#### 2. Run the install script

If you are on a fresh server and logged in as root, use --user flag to create a user and install using that user

	python install.py --develop --user frappe

For the current user:

	sudo python install.py --develop

For production:

	sudo python install.py --production

#### What will this script do?

- Install all the pre-requisites
- Install the command line `bench`
- Create a new bench (a folder that will contain your entire frappe/erpnext setup)
- Create a new site on the bench

#### How do I start ERPNext

1. For development: Go to your bench folder (`frappe-bench` by default) and start the bench with `bench start`
2. For production: Your process will be setup and managed by `nginx` and `supervisor`. [Setup Production](https://frappe.github.io/frappe/user/en/bench/guides/setup-production.html)

---

Help
====

For bench help, you can type

	bench --help

Updating
========

To manually update the bench, run `bench update` to update all the apps, run
patches, build JS and CSS files and restart supervisor (if configured to).

You can also run the parts of the bench selectively.

`bench update --pull` will only pull changes in the apps

`bench update --patch` will only run database migrations in the apps

`bench update --build` will only build JS and CSS files for the bench

`bench update --bench` will only update the bench utility (this project)

`bench update --requirements` will only update dependencies (python packages) for the apps installed

Guides
=======
- [Configuring HTTPS](https://frappe.github.io/frappe/user/en/bench/guides/configuring-https.html)
- [Using Let's Encrypt to setup HTTPS](https://frappe.github.io/frappe/user/en/bench/guides/lets-encrypt-ssl-setup.html)
- [Diagnosing the Scheduler](https://frappe.github.io/frappe/user/en/bench/guides/diagnosing-the-scheduler.html)
- [Change Hostname](https://frappe.github.io/frappe/user/en/bench/guides/how-to-change-host-name-from-localhost.html)
- [Manual Setup](https://frappe.github.io/frappe/user/en/bench/guides/manual-setup.html)
- [Setup Production](https://frappe.github.io/frappe/user/en/bench/guides/setup-production.html)
- [Setup Multitenancy](https://frappe.github.io/frappe/user/en/bench/guides/setup-multitenancy.html)
- [Stopping Production](https://frappe.github.io/frappe/user/en/bench/guides/stop-production-and-start-development.html)


Resources
=======

- [Background Services](https://frappe.github.io/frappe/user/en/bench/resources/background-services.html)
- [Bench Commands Cheat Sheet](https://frappe.github.io/frappe/user/en/bench/resources/bench-commands-cheatsheet.html)
- [Bench Procfile](https://frappe.github.io/frappe/user/en/bench/resources/bench-procfile.html)
