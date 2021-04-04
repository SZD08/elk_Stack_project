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

Load balancing ensures that the application will be highly (available) _____, in addition to restricting (traffic)_____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the (logs) _____ and system (metrics/traffic)_____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name            | Function          | IP Address | Operating System |
|-----------------|-------------------|------------|------------------|
| RedTeam Jumpbox | Gateway           | 10.1.0.5   | Linux            |
| Web-1           | Web server        | 10.1.0.4   | Linux            |
| Web-2           | Web server        | 10.1.0.6   | Linux            |
| Elk-Server      | Monitoring        | 10.2.0.5   | Linux            |

*** PUBLIC IS IS DYNAMIC SHOULD I INCLUDE IT????

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the (jumpbox)_____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 142.117.169.194 (Public IP Address)
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by SSH by the Red Team Jumpbox Machine (10.1.0.5).
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name            | Publicly Accessible | Allowed IP Address |
|-----------------|---------------------|--------------------|
| RedTeam Jumpbox | Yes                 | 142.117.169.194    |
| Web-1           | No                  | 10.1.0.5           |
| Web-2           | No                  | 10.1.0.5           |
| Elk-Server      | No                  | 10.1.0.5           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because i's a time saver and it prevents human errors...
- _TODO: What is the main advantage of automating configuration with Ansible?_

****** HUMAN ERROR****?

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...
Configuring the elk VM with docker:

Installing Docker image
Install python3-pip
Install docker via pip
Increasing virtual memory and setting to use more
Downloading and launching a elk docker container - starts docker and establishes the ports being used.
Enabling docker service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

**Note**: The following image link needs to be updated. Replace `docker_ps_output.png` with the name of your screenshot image file.  


![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png) PROJECT-1-ELK

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

10.1.0.4
10.1.0.6
THE DVWA Machines (web servers)

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

Filebeat 
Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

Filebeat ships log files  You would expect to see log files. EX Unique Visitors by country
Metricbeat ships system-level metrics. You would expect to see in the metric results ; container, cpu, diskio, healthcheck, image, info, memory and network.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the configuration file to /etc/ansible.
- Update the _____ (hosts) file to include the web server machines and the elk machine.
- Run the playbook, and navigate to Elk server VM to check that the installation worked as expected.
http://[Public-IP]:5601/app/kibana#/home


In my case:
http://40.91.97.85:5601/app/kibana#/home


_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._


1- Add the machines to the list of machines Ansible can discover and connect to 
nano /etc/ansible/hosts
		```
		# /etc/ansible/hosts

		[webservers]
		10.1.0.4 ansible_python_interpreter=/usr/bin/python3
		10.1.0.6 ansible_python_interpreter=/usr/bin/python3

		[elk]
		10.2.0.5 ansible_python_interpreter=/usr/bin/python3
		```

2- create a new playbook nano /etc/ansible/install-elk.yml
3- Run the playbook $ ansible-playbook install-elk.yml
4- creating playbooks to install filebeat and metricbeat on the web server vms

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

    Creating the playbook nano filebeat-playbook.yml
    Run it ansible-playbook filebeat-playbook.yml

    Installing Creating playbook for metric beat

Install and edit the config file 

curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > metricbeat-config.yml

nano metricbeat-config.yml
creating the playbook
nano metricbeat-playbook.yml
running the playbook
ansible-playbook metricbeat-playbook.yml
