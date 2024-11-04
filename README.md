# Ansible - Web Server, Proxy Server, Database Server Deployment (Debian)

# Web Deployment Utilizing Ansible

Multiverse Project - An Ansible autmation script to deploy numerous web servers, proxy servers, and databases based on your needs. This project is currently just utilzing Debian powered virtual machines to demonstrate its' usability. I have chose to host my virtual machines using VMware to save cost but the script can be used with virtual machinese hosted on cloud providers such as AWS, Azure, and Google Cloud as long as the OS on the hosts are Debian unless the script is modified. This script also utilizes a project repository hosted on GitHub. You can choose to host your own project, however, keep in mind the file structure NGINX expects to host a web server. In my case, I used the project found here: [inventory-app2](https://github.com/jbiehl88/inventory-app2).

## Getting Started

1. Ansible installed on a linux based device that will be your dedicated Ansible Controller, either a hosted device or you local device.
2. Either manually create an ssh key pair and add the public key to your hosts in trusted devices OR when creating your hosts, make sure the public key is added in configuration to allow Ansible to communicate to the hosts. There is an untested ansible script for the handshake included in this project but for my instance, the VMs had to be a manually configured handshake.
3. Adjust the `inventory.ini` file to include your created VM hosts. For my use, since I was using my local resources, I kept things minimal with one database server, one proxy server, and three web servers.
4. Once the ssh handshake and adjustments made to your inventory file are complete, you can test your connection to the hosts by running the `ping.yml` playbook. (`ansible-playbook -i inventory.ini ping.yml`)
5. Next step is to go into the roles directory -> web_server directory and create a directory called `vars`. Within the vars directory, create a file called `main.yml`. This file contains sensitive data so make sure it is included into your `gitignore` file. The file should be createed to include the following:\
  `db_host:`(host ip of your selected database server)\
  `db_user:`(username created when creating the database)\
  `db_password:`(password for database)\
  `db_name:`(name of the database you wish to connect with)\
  `db_port:` `5432` (default for postgres)\
  `api_server:` `"http://{{ ansible_default_ip4.address }}"` this is dynamically gathered by ansible.
6. Change directory to the `root` of your project, and run the playbook labeled `setup.yml` - (`ansible-playbook -i inventory.ini setup.yml -u <username_on_host_machine> --ask-become-pass`)`
7. The script will ask for a password, this password will be the `root` password created during the VM creation using the Debian OS.
8. If that playbook runs successfully, run the playbook labeled `setup_proxy.yml` - (`ansible-playbook -i inventory.ini setup_proxy.yml -u <username_on_host_machine> --ask-become-pass`)
9. If that playbook also runs successfully, a test script labeled `test.yml` can be run to verify `NGINX` is runing on the web and proxy servers, `POSTGRES` is running on the database server, and the `API` servers are running in pm2 locally on the web servers.
10. ****Other configurations may be required based on the project you are wanting to host from your github as well as the required structure that NGINX expects within your selected project****

## My Project Procedure
### Ansible
- Install Ansible on controller (linux required).
- Host 3 VMs (minimum) either locally or on a cloud provider.
- Create ssh handshake from controller to the other hosts.
- Adjust the inventory.ini file to your requirements.
- Run ping.yml to test connections to your host VMs.
- Create a directory called vars in the roles/web_server directory.
- Create a file called main.yml in the vars directory created above and add the key/values of db_host, db_user, db_password, db_name, db_port, api_server (see getting started #5 above)
- Change directory to the root of your project and run the playbook labeled setup.yml.
- Upon successful completion of the setup.yml playbook, run the playbook labeled setup_proxy.yml
- If both playbooks run successfully, you can test that all processes are running by running the playbook labeled test.yml.

### Web Application - User Story
- From the landing page, you are presented with a full list of available items.
- Click on any item on the landing page and you will be presented with a new view of that item including all the details of that item as well as an option to go back to the landing page, update the item, or delete item.
- Clicking on the update button, you will see a new view with 5 form fields that are prefilled with the existing information for easy editing. Once complete with making your changes, click submit and you will be returned to the landing page with the updated information present on the item you updated.
- Clicking on the delete button will return you to the landing page with that specific item no longer in the list on the landing page.
- At the top of the page there are two buttons, one to add an item to the database and landing page, and the other to search for items based on your input.
- Clicking on the Add Item button, will take you to a new view that will present you with 5 form fields, name, description, price, category, and image. All fields are required for you to submit. The Price field allows only numbers to be entered (ex. 500.00 or 500). Finally the image field only allows for URLs to be submitted (ex. http://something.com). -Clicking on the Search button with bring you to another view that has one form field and a submit button allowing you to search for items based on partial information, such as, Mens or Gold. Once the submit button is pressed, any item that contains the entered information will be displayed on the screen including all the details of that item.

## Built With

- Ansible 2.14.16
- Python 3.11.2
- Jinja 3.1.2
- NGINX
- PM2
- Node
- Express
- React
- Sequelize

## Backlog

Items to be implemented in future developments:

- Further development of database script to automate the creation of the database tables and seeding the data.

## Author
- [Jordan Biehl](https://github.com/jbiehl88)
