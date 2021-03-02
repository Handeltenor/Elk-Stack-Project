# Elk-Stack-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!(BenSDen.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook created may be used to install only certain pieces of it, such as Filebeat.

  -  The following ansible playbooks are required to install the DVWA'a and the Elk-server
   - [pentest.yml](Playbooks/Pentest-Playbook.yml)
   - [Elk-Playbook.yml](Playbooks/Elk-Playbook.yml)
   - [Filebeat-Playbook.yml](Filebeat-Playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly overused, in addition to restricting specific access to the network.
- The purpose of having a load balancer is to protect the servers from a DoS attack, while at the same time, distrubutes or "balances" the traffic between the servers.  Additionally, the purpose of the Jumpbox, is to protects the VM's created, from being accessed by the public.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files from the log and system function and its performance.
- Filebeat monitores log files
- Metricbeat records metrics from the server and Operating System.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux 18.04      |
| Web 1    | Server   | 10.0.0.7   | Linux 18.04      |
| Web 2    | Server   | 10.0.0.6   | Linux 18.04      |
| ELKVM    | Elkserver| 10.1.0.4   | Linux 18.04      |                  

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 137.135.120.143

Machines within the network can only be accessed by my public IP address
- The Jumpbox is the only machine permitted to connet outside of the network

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No.                 | Private IP address.  |
| Web 1    | No.                 | Private IP 10.0.0.7  |
| Web 2    | No.                 | Private IP 10.0.0.6  |
| Elk Serv.| No.                 | SSH to IP 10.1.0.4.  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
- it can be run from the command line and the created scripts will run uniform, no matter where they are running from.

The playbook implements the following tasks:
1. Install Docker.io and pip3
2. Increases VM memory
3. Download and configure the Elk Docker Container. 
4. Assigns Public Ports. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
!(Images/PS output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 IP : 10.0.0.7
- Web 2 IP : 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects the log data and their source (location).   

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files.
- Update the configuration files to include the Private IP of the ELK-Server to the Kibana Section of the Configuration File.
- Run the playbook and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected.


- _Which file is the playbook? Where do you copy it?
- The Playbook files are:
- [Elk-Playbook.yml](Playbooks/Elk-Playbook.yml) : used to install the Elk-Server.It is copied to the /etc/ansible/files directory.
- _Which file do you update to make Ansible run the playbook on a specific machine?
- The /etc/ansible/hosts.cfg file needs to be updated in order to make Ansible run the playbook on a specific machine.
-  How do I specify which machine to install the ELK server on versus which to install Filebeat on?
-  In /etc/ansible/hosts you tell it where you want eachto be installed ElkServers or FileBeat.
- _Which URL do you navigate to in order to check that the ELK server is running?
- In order to make sure that the Elk server is running, we neet to navigate to the http://publicip(elkserver):5601 URL

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
1. ssh azdmin@JumpBox(PrivateIP)
2. sudo docker container list -a - Locate the ansible container
3. sudo docker start (boring_robinson)
4. sudo docker attach (boring_robinson)
5. cd /etc/ansible
6. ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
7. cd /etc/ansible/
8. ansible-playbook beats-playbook.yml (Installs and Configures Beats)
9. Open a new browser on Personal Workstation, navigate to (ELK-Server-PublicIP:5601/app/kibana) - This will bring up Kibana Web Portal
