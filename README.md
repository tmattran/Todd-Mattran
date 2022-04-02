### Automated ELK Stack Deployment

### The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/tmattran/ELK-Stack-Project-1/blob/main/Diagrams/XCorp%20Red%20Team%20Topography.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible file may be used to install only certain pieces of it, such as Filebeat.

Enter the playbook file:

![alt text](https://github.com/tmattran/ELK-Stack-Project-1/blob/main/Ansible/Install-ELK.yml)

### This document contains the following details:

Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly protected, in addition to restricting access to the network.

### What aspect of security do load balancers protect? 
Load balancers protect servers and distribute traffic evenly across the servers to mitigate DDoS attacks.

### What is the advantage of a jump box?
The jump box is the only way to access the DVWA machines within the local network via SSH key on 22 making the network secure from external attacks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

### What does Filebeat watch for? 
Filebeat monitors the log files or location you specify, collects log events and forwards them to the Elasticsearch or Logstash.

### What does Metricbeat record?
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### The configuration details of each machine may be found below.

|: Name                    |: Function            |: IP Address          |: Operating System
|: Jump Box Provisioner    |: Gateway             |: 52.238.31.167       |: Linux
|: Web 1                   |: Gateway             |: 13.86.154.148       |: Linux
|: Web 2                   |: Gateway             |: 13.86.154.148       |: Linux
|: Web 3                   |: Gateway             |: 13.86.154.148       |: Linux

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

52.238.31.167\
13.86.154.148

### Machines within the network can only be accessed by Jump Box Provisioner.

### A summary of the access policies in place can be found in the table below.
Name                    Publicly Accessible     Allowed IP Addresses
Jump Box Provisioner    Yes                     52.238.31.167
XCorp Red Team LB       Yes                     13.86.154.148
Web-1                   No                      10.0.0.5, 10.1.0.4, 13.86.154.148
Web-2                   No                      10.0.0.6, 10.1.0.4, 13.86.154.148
Web-3                   No                      10.0.0.10, 10.1.0.4, 13.86.154.148

### Elk Configuration

### Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

### What is the main advantage of automating configurations with Ansible?

Ansible is a provisioner that is utilized to automatically deliver Infrastructure as Code (IAC), which drastically reduces the potential for human error and simplifies the process of configuring potentially thousands of machines identically all at once.

### The playbook implements the following tasks:

Configure Elk VM with Docker on the elk host with the remote user azadmin.\
Install docker.io, install python3-pip, install Python Docker module.\
Increase virtual memory.\
Use more memory.\
Download and launch a docker elk container on restart.\
Enable service docker on boot.\
The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

### Target Machines & Beats

### This ELK server is configured to monitor the following machines:

Web-1, IP Address 10.0.0.5\
Web-2, IP Address 10.0.0.6\
Web-3, IP Address 10.0.0.10

### We have installed the following Beats on these machines:

filebeat-config.yml\
filebeat-playbook.yml\
metricbeat-config.yml\
metricbeat-playbook.yml  

### These Beats allow us to collect the following information from each machine:

Filebeat consists of two main components “inputs” and “harvesters” which work together to tail files and send event data to the output you specify. The harvester is responsible for reading the content of a single file. The input is responsible for managing the harvesters and finding all sources to read from.

Metricbeat collects metrics from the operating system and services running on the servers. It takes the metrics and statistics that it collects and ships them to the output you specify, such as Elasticsearch or Logstash.

### Using the Playbook

### In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

### SSH into the control node and follow the steps below:

Copy the filebeat-config.yml file to root@c2e11045c8f5:/etc/ansible/files/filebeat-config.yml\
Update the ansible hosts file to include the following webservers:\
[webservers]\
= 10.0.0.5 ansible_python_interpreter=/usr/bin/python3\
= 10.0.0.6 ansible_python_interpreter=/usr/bin/python3\
= 10.0.0.10 ansible_python_interpreter=/usr/bin/python3

[elk]\
= 10.1.0.4 ansible_python_interpreter=/usr/bin/python3

Run the playbook, and navigate to http://20.25.25.104:5601/app/kibana to check that the installation worked as expected.

### Answer the following questions to fill in the blanks:

### Which file is the playbook? Where do you copy it?

= filebeat-playbook.yml is the playbook.\
= The filebeat playbook is copied inside the Jump Box Provisioner VM- root@c2e11045c8f5:/etc/ansible/roles/filebeat-playbook.yml

### Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

= The root@c2e11045c8f5:/etc/ansible/hosts file has to be updated in order to run the playbook on a specific machine.\
= Once you navigate into the /etc/ansible/hosts, nano into the hosts file and add the [webservers] that will be configured with filebeat and the [elk] VM which will be configured with the ELK Stack.

### Which URL do you navigate to in order to check that the ELK server is running?

= http://20.25.25.104:5601/app/kibana

### The commands needed to download a playbook are:

ssh azadmin@jump-box-ip-address\
sudo docker container list -a (command will list the name of the ansible container).\
sudo docker start (add container name).\
sudo docker attach (add container name).\
cd /etc/ansible/\
nano /etc/ansible/hosts/ (once inside of the hosts file, configure [webservers] and [elk]).\
curl https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb > /etc/filebeat/filebeat.yml\
dpkg -i filebeat-7.4.0-amd64.deb\
nano filebeat-playbook.yml (add the filebeat playbook configuration).\
ansible-playbook filebeat-playbook.yml
