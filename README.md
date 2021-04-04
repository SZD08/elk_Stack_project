# elk_Stack_project

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Elk_Stack_Diagram](https://github.com/SZD08/elk_Stack_project/blob/main/Diagrams/Elk-STACK-diagram.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topologie
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics.

The configuration details of each machine may be found below.

| Name            | Function          | IP Address | Operating System |
|-----------------|-------------------|------------|------------------|
| RedTeam Jumpbox | Gateway           | 10.1.0.5   | Linux            |
| Web-1           | Web server        | 10.1.0.4   | Linux            |
| Web-2           | Web server        | 10.1.0.6   | Linux            |
| Web-3           | Web server        | 10.1.0.7   | Linux            |
| Elk-Server      | Monitoring        | 10.2.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 142.117.169.194 (Public IP Address)

Machines within the network can only be accessed by SSH by the Red Team Jumpbox Machine (10.1.0.5).

A summary of the access policies in place can be found in the table below.

| Name            | Publicly Accessible | Allowed IP Address |
|-----------------|---------------------|--------------------|
| RedTeam Jumpbox | Yes                 | 142.117.169.194    |
| Web-1           | No                  | 10.1.0.5           |
| Web-2           | No                  | 10.1.0.5           |
| Web-3           | No                  | 10.1.0.5           |
| Elk-Server      | No                  | 10.1.0.5           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it saves time and it prevents human errors.

The playbook implements the following tasks:

 - Installing Docker image
 - Install python3-pip
 - Install docker via pip
 - Increasing virtual memory 
 - Downloading and launching a elk docker container 
 - Enabling docker service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker_PS_Output](https://github.com/SZD08/elk_Stack_project/blob/main/Images/docker_ps_output.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
- 
##### THE DVWA Machines (web servers)
 - 10.1.0.4
 - 10.1.0.6


We have installed the following Beats on these machines:

 - Filebeat 
 - Metricbeat

Filebeat ships log files. You would expect to see logs like the ?
Metricbeat ships system-level metrics. You would expect to see in the metric results ; container, cpu, diskio, healthcheck, image, info, memory and network.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the configuration file to /etc/ansible.
- Update the hosts file to include the web server machines and the elk machine.
- Run the playbook, and navigate to Elk server VM to check that the installation worked as expected.
  - http://[Public-IP]:5601/app/kibana#/home
  - In my case: http://40.91.97.85:5601/app/kibana#/home

### Commands to deploy the ELK server

1. Add the machines to the list of machines Ansible can discover and connect to 
```bash
$ nano /etc/ansible/hosts
```
		```
		# /etc/ansible/hosts

		[webservers]
		10.1.0.4 ansible_python_interpreter=/usr/bin/python3
		10.1.0.6 ansible_python_interpreter=/usr/bin/python3

		[elk]
		10.2.0.5 ansible_python_interpreter=/usr/bin/python3
		```

2. creating the playbook 
```bash
nano /etc/ansible/install-elk.yml
```
   [Elk Playbook File](https://github.com/SZD08/elk_Stack_project/blob/main/Scripts/Ansible/elk-server-playbook.yml)
   
4. Run the playbook 
```bash 
$ ansible-playbook install-elk.yml
```
6. creating playbooks to install filebeat and metricbeat on the web server vms

Download and copy the Filebeat configuration files to the /etc/ansible directory
$ curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml

edit the config file as needed $ nano filebeat-config.yml

    - Scroll to line #1106 and replace the IP address with the IP address of the ELK machine.

      ```bash
      output.elasticsearch:
      hosts: ["10.2.0.5:9200"]
      username: "elastic"
      password: "changeme"
      ```

    - Scroll to line #1806 and replace the IP address with the IP address of the ELK machine.

      ```   
      setup.kibana:
      host: "10.2.0.5:5601"
      ```
    **SHOULD I INCLUDE THE PLAYBOOK?**

7. Creating the playbook
```bash
nano filebeat-playbook.yml
 ```
 
[Filebeat Playbook File] (https://github.com/SZD08/elk_Stack_project/blob/main/Scripts/Ansible/filebeat-playbook.yml)
 
8. Run the playbook 
 ```bash
ansible-playbook filebeat-playbook.yml
   ```
9. Installing MetricBeat

```bash 
# Installing and editing the configuration file
curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > metricbeat-config.yml
# Making changes to the configuration file
nano metricbeat-config.yml
# Creating the playbook 
nano metricbeat-playbook.yml
```
[Metricbeat Playbook File] (https://github.com/SZD08/elk_Stack_project/blob/main/Scripts/Ansible/metricbeat-playbook.yml)

```bash
# Running the playbook
ansible-playbook metricbeat-playbook.yml
```


