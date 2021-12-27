## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/Project 1 diagram.jpeg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  ---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network. What aspect of security do load balancers protect? What is the advantage of a jump box?
Load balancers ensure the availability of your network(s) remain uncompromised, reducing the risk of being taken down by a DDoS Attack, by distributing traffic evenly across servers, ensuring they do not get overloaded. 
A jump box protects your Virtual Networks by controlling access to other machines, only allowing connections from specific IP addresses, and forwarding to those machines. It sits in front of other machines not exposed to the public internet, similar to that of a gateway router.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic. What does Filebeat watch for?: Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
What does Metricbeat record?: Metricbeat records metrics from the system and services running on the server, it then takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function              | IP Address       | Operating System |
|----------|-----------------------|------------------|------------------|
| Jump Box | Gateway               | 10.0.0.4         | Linux            |
| Web1     | Web Server (DVWA)     | 10.0.0.5         | Linux            |
| Web2     | Web Server (DVWA)     | 10.0.0.6         | Linux            |
| ELK      | Access to Kibana      | 10.1.0.4         | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 76.68.14.161 (My Public IP)

Machines within the network can only be accessed by Jumpbox.
Which machine did you allow to access your ELK VM? What was its IP address?: JumpBox Provisioner - 52.255.146.241/10.0.0.4   


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 52.255.146.241       |
| Web1VM   | Yes (via LB)        | 10.0.0.5             |
| Web2VM   | Yes (via LB)        | 10.0.0.6             |
| ELKVM    | Yes (port 5601)     | 104.42.37.213	      |		

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it negated manual configuration of the ELK machine, resulting in a more streamlined process with less chance of human error.

What is the main advantage of automating configuration with Ansible?: Ansible is a configuration management platform that automates storage, servers, and networking. When you use Ansible to configure these components, difficult manual tasks become repeatable and less vulnerable to error.

The playbook implements the following tasks:
The playbook installs docker.io on the ELK VM.
The playbook then installs Python on the ELK VM
The playbook increases virtual memory
The playbook then uses the increased virtual memory
The playbook downloads, installs and launches a docker ELK container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


### Target Machines & Beats
This ELK server is configured to monitor the following machines: Web 1 (10.0.0.5) & Web 2 (10.0.0.6)

We have installed the following Beats on these machines:
Filebeat & Metricbeat.

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible
- Update the filebeat-config.yml file to include hosts 10.1.0.4:9200 (Elasticsearch Output) and "10.1.0.4:5601"(Kibana)
- Run the playbook, and navigate to http://104.42.37.213:5601/app/kibana to check that the installation worked as expected.
Which file is the playbook?: filebeat-playbook1.yml
Where do you copy it? /etc/filebeat/filebeat.yml
Which file do you update to make Ansible run the playbook on a specific machine? Hosts (/etc/ansible/hosts)
How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
Which URL do you navigate to in order to check that the ELK server is running?: http://104.42.37.213:5601/app/kibana

As a Bonus provide the specific commands the user will need to run to download the playbook, update the files, etc.: Ansible-playbook [select the specified playbook]
Ansible-playbook filebeat-playbook.yml
Ansible-playbook metricbeat-playbook.yml


