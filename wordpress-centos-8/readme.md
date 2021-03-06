# To install WordPress on a low-RAM CentOS 8 cloud server using Ansible 2.9

## Have the SSL certificate files

* example_com.key
* example_com.crt
* example_com.ca-bundle

## Have Ansible installed on the controller Linux server:
On CentOS 8:

	sudo dnf install python3 python3-pip
	pip3 install ansible --user

## Setup SSH key login to the slave server:
	Add this to the ~/.ssh/config file:
---
	Host <remote host nickname>
	Hostname <remote DNS or IP address>
	user <remote username for ssh>
	identityfile ~/.ssh/<private key file>.pem
---

Copy your private key for the SSH connection to

	~/.ssh/<private key file>.pem
	
Test the SSH connection:

	ssh <remote host nickname>

## Configure the Ansible hosts file

Edit /etc/ansible/hosts to add a "wordpress" group, your managed server, and an Ansible variable to tell it where the Python interpreter is. (Python 2 is the default for Ansible, but CentOS 8 uses Python 3.)

---
	[wordpress]
	<ssh nickname / managed server address>
	[wordpress:vars]
	ansible_python_interpreter=/usr/bin/python3
---

## Create an Ansible vault to store secrets

Make a plain text file for the the database password:
	
secret.yml:

---
	wordpress_database_user_password: <password-here>
---

Then encrypt the plain text file:

	ansible-vault create secret.yml
		
Encrypt the SSL files using the same password:

	ansible-vault encrypt <example_com>.key <example_com>.crt <example_com>.ca-bundle

## Edit the configuration file

	cp configure.default.yaml configure.yaml
	nano configure.yaml

## Run the playbook

	ansible-playbook main.yaml --ask-vault-pass

## Setup WordPress

In a web browser, go to your domain or the managed machine's IP address:

	https://example.com

In the browser:
* Pick the language.
* Leave the database name "wordpress".
* Enter "wordpress" for the username.
* Enter the database password that you entered in secret.yml
* Leave the Database Host as localhost.
* Leave the Table Prefix "wp_"
* Click to run the installation
* Enter the information needed.




