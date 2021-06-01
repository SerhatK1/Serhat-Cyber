## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

NetworkDiagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Dmn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.
 * What aspect of security do load balancers protect? 
..*It helps prevent overloading servers as well as optimizes productivity and maximizes uptime. It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attack.

 * What is the advantage of a jump box?
..* -Jump-box are highly secured computers that are never used for non-admin tasks.Throughout the years, jump-box has improved into an even more comprehensive/lock-down secure admin workstation to decrease the chances of hackers/malware infection

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
..* What does Filebeat watch for?
It monitors the log files/locations that you specify and forward them to Elasticsearch for indexing
..* What does Metricbeat record?
It records metrics/statistics data and transports them to the otput that you specify through elastic search

The configuration details of each machine may be found below.

| Name   | Function   | IP Address | Operating System |
|--------|------------|------------|------------------|
| Red-VM | Gateway    | 10.1.0.4   | Linux            |
| Web-1  | Server     | 10.1.0.7   | Linux            |
| Web-2  | Server     | 10.1.0.8   | Linux            |
| Elk-VM | Monitoring | 10.0.2.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box red-vm machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
..* Add whitelisted IP addresses_

Machines within the network can only be accessed by Red-VM.
* Which machine did you allow to access your ELK VM? What was its IP address?
..* Red-VM - 20.36.34.172

A summary of the access policies in place can be found in the table below.

| Name   | Publicly available? | Allowed IP address |
|--------|---------------------|--------------------|
| Red-VM | yes                 | 20.36.34.172       |
| Web-1  | no                  | 10.1.0.4           |
| Web-2  | no                  | 10.1.0.4           |
| Elk-VM | no                  | 10.1.0.4           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
* What is the main advantage of automating configuration with Ansible?_
..* YAML Playbooks: It is the best alternative for configuration management.
..* Automate complex multi-tier IT application environments.

The playbook implements the following tasks:
..* Install docker.io
..* Install python3-pip
..* Install docker via pip
..* Increase vitual memory
..* Use more memory
..* Download and launch a docker elk container


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
..* Web-1 10.1.0.7
..* Web-2 10.1.0.8

We have installed the following Beats on these machines:
..* 10.2.0.4
* Specify which Beats you successfully installed
..* Filebeat
..* Metricbeat

* These Beats allow us to collect the following information from each machine:In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
..* Filebeat collects log data and shows them in the monitoring clusters
..* Metricbeat collects metrics and statistics and shows them in the output specified, for example Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
..* Copy the filebeat config file to /etc/ansible/roles.
..* Update the filebeat-playbook.yml file to include 1106 and 1806
..* Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
..* Which file is the playbook? Where do you copy it?
filebeat-playbook.yml - /etc/ansible/roles
..* Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
You update the hosts file to include IP adress of host vm
..* Which URL do you navigate to in order to check that the ELK server is running?

..* As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

---
  - name: installing and launching filebeat
	   hosts: webservers
       become: true
       tasks:

	   - name: download filebeat deb
  	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

	   - name: install filebeat deb
  	     command: dpkg -i filebeat-7.7.1-amd64.deb

	   - name: drop in filebeat.yml
  	     copy:
   	       src: /etc/ansible/roles/filebeat-config.yml
   	       dest: /etc/filebeat/filebeat.yml

	   - name: enable and configure system module
  	     command: filebeat modules enable system

	   - name: setup filebeat
  	     command: filebeat setup

	   - name: start filebeat service
  	    command: service filebeat start
