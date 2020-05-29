# To install WordPress on a low-RAM CentOS 8 cloud server using Ansible 2.9

## Have the files for an SSL certificate

* example_com.key
* example_com.crt
* example_com.ca-bundle

## Have Ansible installed on a master server:
On CentOS 8:

	sudo dnf install python3 python3-pip

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

Edit /etc/ansible/hosts to add a "wordpress" group, your slave server, and an Ansible variable to tell it where the Python interpreter is. (Python 2 is the default, but CentOS 8 uses Python 3.)

---
	[wordpress]
	<ssh nickname / slave server address>
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

## Run it

Run the main Ansible playbook with something like:
	ansible-playbook main.yaml --ask-vault-pass

