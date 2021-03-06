# Project-One-ELK-Stack
Cybersecurity ELK Stack Project
---

## ELK-Stack-Project
**Automated ELK Stack Deployment**
 
The files in this repository were used to configure the network depicted below.

![vNet Diagram](https://github.com/Bas9711/fproject-1/blob/dfb1ca7ae94ecda2435a7f4f7c6fa59ca5ad8210/image%20(1).png)
 
These files have been tested and used to generate an automated ELK Stack Deployment on Azure. They can be used to either recreate the entire deployment figured below. Otherwise, select portions of the YAML files may be used to install only certain pieces of it, for example, Filebeat and Metricbeat.

  - [install-elk.yml](https://github.com/Bas9711/fproject-1/blob/095052fb04bce306891b6d0abf55b22e05834c4a/CONFIG%20FILES/install-elk.yml
)
  - [filebeat-config.yml](https://github.com/Bas9711/fproject-1/blob/095052fb04bce306891b6d0abf55b22e05834c4a/CONFIG%20FILES/filebeat-configuration.yml)

  - [filebeat-playbook.yml](https://github.com/Bas9711/fproject-1/blob/095052fb04bce306891b6d0abf55b22e05834c4a/CONFIG%20FILES/filebeat-playbook.yml)
 
  - [metricbeat-config.yml](https://github.com/Bas9711/fproject-1/blob/095052fb04bce306891b6d0abf55b22e05834c4a/CONFIG%20FILES/metricbeat-configuration.yml
)
  - [metricbeat-playbook.yml](https://github.com/Bas9711/fproject-1/blob/095052fb04bce306891b6d0abf55b22e05834c4a/CONFIG%20FILES/metricbeat-playbook.yml)
 
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build
 
### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.

- According to [Azure security baseline for Azure Load Balancer](https://bit.ly/3AnSRPV), the load balancer's main purpose is to distribute web traffic across multiple servers. In our network, the load balancer was installed in front of the VM to 
   - protect Azure resources within virtual networks.
   - monitor and log the configuration and traffic of virtual networks, subnets, and NICs.
   - protect critical web applications
   - deny communications with known malicious IP addresses
   - record network packets
   - deploy network-based intrusion detection/intrusion prevention systems (IDS/IPS)
   - manage traffic to web applications
   - minimize complexity and administrative overhead of network security rules
   - maintain standard security configurations for network devices
   - document traffic configuration rules
   - use automated tools to monitor network resource configurations and detect changes


> What is the advantage of a jump box?
- A Jump Box acts as a bridge between two networks which provides a controlled access point. We can block the public IP address thtat is affiliated with the Virutal Machine. Not only does it help to improve security but also prevents all Azure Virutal Mahcines to be expose to the public eye.

Integrating an Elastic Stack server allows us to easily monitor the vulnerable VMs for changes to their file systems and system metrics such as privilege escalation failures, SSH logins activity, CPU and memory usage, etc.

> What does Filebeat watch for?
- A Filebeat helps keep things straight to the point and easy. By offering a low maintenance memory blueprint to forward and centralize logs, files and watches for changes as well.

> What does Metricbeat record?
- Metricbeat helps watches servers by gathering metrics from the system and running on the server so it keeps track of the machine metrics and stats.

The configuration details of each machine may be found below.
 
| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump-Box | Gateway  | 40.78.46.70; 10.0.0.5  | Linux    |
| Web-1        |webserver    | 10.0.0.5    | Linux            |
| Web-2        |webserver    | 10.0.0.6   | Linux            |
| ELKServer    |Kibana       |  10.1.0.4  | Linux       |
| RedTeam-LB|Load Balancer| 13.90.156.166| DVWA            |
 
In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box. The load balancer's targets are organized into availability zones: Web-1 + Web-2


### Access Policies
 
The machines on the internal network are not exposed to the public Internet.
 
Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 20.210.140.233 Machines within the network can only be accessed by SSH from Jump Box.
 
A summary of the access policies in place can be found in the table below.
 
| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump-Box | Yes                 | 40.78.46.70; 10.0.0.4|
| ELKServer| No                  |  10.1.0.4        |
| DVWA 1   | No                  |  10.0.0.5        |
| DVWA 2   | No                  |  10.0.0.6        |

 
---


Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
  	- With this feature you can launch a multiple of servers by only using just one playbook.

The playbook implements the following tasks:
- Install docker.io
- Install Python-pip
- Install docker container
- Launch docker container: elk
- Command: sysctl -w vm.max_map_count=262144


Target Machines & Beats

This ELK server is configured to monitor the following machines:

- List the IP addresses of the machines you are monitoring:-
    	- Web-1(10.0.0.5) Web-2(10.0.0.6)

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed:-
    	-  Filebeat
    	-  Metricbeat

These Beats allow us to collect the following information from each machine:

   	- Filebeat monitors also known as log files, collect log events then which forwards them to Elasticsearch or Logstash for indexing.
   	- Metricbeat not only collects metrics from the operating system but from the services running on the server as well. 

 Using the Playbook-install-elk.yml

     In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

   - Copy the playbook file to /etc/ansible.
   - Update the the configuration file to include the webservers and ElkVM (private Ip address. 
   - Run the playbook, and navigate to ElkVM to check that the installation worked as expected.
/etc/ansible/host should include:

[webservers]
[10.0.0.5] ansible_python_interpreter=/usr/bin/python3
[10.0.0.6] ansible_python_interpreter=/usr/bin/python3


[elk]
[10.1.0.4] ansible_python_interpreter=/usr/bin/python3

Run the playbook, and SSH into the Elk vm, then run docker ps to check that the installation worked as expected.
Playbook: install_elk.yml Location: /etc/ansible/install_elk.yml
Navigate to  http://[your.ELK-VM.External.IP]:5601/app/kibana to confirm ELK and kibana are running.  You may need to try from multiple web browsers
Click 'Explore On Your Own' and you should see the following:

 <img width="960" alt="Kibana_dashboard" src="https://user-images.githubusercontent.com/66395625/94220160-fddae580-fead-11ea-9c17-8a789bf1522c.PNG">

 Using the Playbook-filebeat-playbook.yml

 - Copy the filebeat.yml file to the /etc/ansible/files/ directory.
 - Update the configuration file to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file.
 - Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the filebeat setup, and start the filebeat service.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the metricbeat setup, and start the metricbeat service. 
 - Run the playbooks, and navigate back to the installation page on the ELk-Server GUI, click the check data on the Module Status
 
   The commands needed to run the Ansible configuration for the Elk-Server are:

	- ssh azadmin@JumpBox(Public IP)
	- sudo docker container list -a (locate your ansible container)
	- sudo docker start container (name of the container)
	- sudo docker attach container (name of the container)
cd /etc/ansible/
	- ansible-playbook elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server
	- cd /etc/ansible/roles/
	- ansible-playbook filebeat-playbook.yml (installs Filebeat and Metricbeat)
	- open a new web browser (http://[your.ELK-VM.External.IP]:5601/app/kibana) This will bring up the Kibana Web Portal
	- check the Module status for file beat and metric beat to see their data receiving.

** You will need to ensure all files are properly placed before running the ansible-playbooks.
