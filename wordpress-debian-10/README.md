# Configuration
Edit configure.yaml to match your domain and email address.
 
Create a secret.yml file in the following format:
 
 	wordpress_database_user_password: password-goes-here

Copy the HTTPS certificate, secret key, and CA certificate to the folder with the main.yaml file.

Encrypt the secret.yml file, HTTPS certificate, secret key, and CA certificate with ansible-vault. Use the same password for each.

	ansible-vault create secret.yml
	ansible-vault encrypt site.key site.crt site.ca-bundle

On the source server, edit /etc/ansible/hosts to add a "wordpress" group and put your server in it:

	[wordpress]
	<target server address>
	
Run the main playbook:

	ansible-playbook main.yaml --ask-vault-pass