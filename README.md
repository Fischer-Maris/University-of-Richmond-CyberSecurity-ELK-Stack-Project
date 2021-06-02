## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ScreenShot](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Diagrams/Network-Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Playbookscripts file may be used to install only certain pieces of it, such as Filebeat.

[Ansible Playbook](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/Ansible-Playbook)

[ELK Installation Playbook](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/ELK-Installation)

[Filebeat Playbook](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/Filebeat-Playbook.txt)

[Metricbeat](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/Metricbeat-Playbook)

[Filbeat Configuration](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/Filebeat-Config)

[Metricbeat Configuration](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/Metricbeat-Config)

[Hosts File](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/hosts)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.

What aspect of security do load balancers protect?
Answer: Availability, Web Traffic, Web Security

What is the advantage of a jump box?
Answer: Automation, Security, Network Segmentation, Access Control

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat monitors, locates and ships log data.
- Metricbeat outputs metrics and statistics to help monitor servers.

The configuration details of each machine may be found below.

| Name        | Function   | IP Address | Operating System |
|-------------|------------|------------|------------------|
| Jump Box    | Gateway    | 10.0.0.1   | Linux            |
| Web-1       | Webserver  | 10.0.0.7   | Linux            |
| Web-2       | Webserver  | 10.0.0.8   | Linux            |
| Django-VM-1(ELKserver) | Monitoring | 10.1.0.4   | Linux            |
  
  
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Elk Sever machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Workstation Public IP through TCP 5601

Machines within the network can only be accessed by Workstation and JumpBox.
Which machine did you allow to access your ELK VM? What was its IP address?
- Jump-Box-Provisioner IP:10.0.0.1 via SSH port 22
- Workstation Public IP via port TCP 5601
A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses              |
|------------|---------------------|-----------------------------------|
| Jump Box   | No                  | Workstation Public IP on SSH 22   |                 
| Web-1      | No                  | 10.0.0.7 on SSH 22                |
| Web-2      | No                  | 10.0.0.8 on SSH 22                |
| ELK-Server | No                  | Workstation Public IP on TCP 5601 |
| Load Balancer      | No                  | Workstation Public IP on HTTP 80  |
                                                         

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible lets you quickly and easily deploy multitier apps. You list the tasks Needed to be done by writing a playbook, and Ansible will figure out how to get your systems to the state you want them to be in.

The playbook implements the following tasks:

- Specify a different group of machines as well as a different remote user
```html
-name: Config elk VM with Docker:
 hosts: elk
 remote_user: azadmin
 tasks:
 ```
 
 - Increase System Memory :
 ```html
- name: Use more memory
 sysctl:
   name: vm.max_map_count
   value: '262144'
   state: present
   reload: yes
```

- Install the following services:
```html
   `docker.io`
   `python3-pip`
   `docker`, which is the Docker Python pip module.
```

- Launching and Exposing the container with these published ports:
```html
 `5601:5601` 
 `9200:9200`
 `5044:5044`
```

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![markdown Logo](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Images/Progect%201%20ELK%20server%20contianer.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 : 10.0.0.7
- Web-2 : 10.0.0.8

We have installed the following Beats on these machines:
- ELK Server, Web-1 and Web-2
- The ELK Stack Installed are: FileBeat and MetricBeat

These Beats allow us to collect the following information from each machine:
- Filebeat: Log events
- Metricbeat: Metrics and system and system statistics

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

For ELK installation VM Configuration:

- Copy the [ Ansible ELK installation and VM Configuration](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/ELK-Installation)
- Run the Playbook using this command:```  ansible-playbook install-elk.yml ```
- Verify with ```  ansible-playbook install-elk.yml ``` again and everything should say "ok" in green.

- _Which file is the playbook? Where do you copy it?_

Answer: A playbook file is a .yml, and you copy them them to source and destination
 ```html - name: drop in filebeat.yml
      copy:
        src: /etc/ansible/files/filebeat-config.yml
        dest: /etc/filebeat/filebeat.yml
```
 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

Answer: Update the [host file](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Ansible/hosts) . To specify you add the privite IP under elk not webservers in the hosts file.

- _Which URL do you navigate to in order to check that the ELK server is running?

![ScreenShot](https://github.com/Fischer-Maris/University-of-Richmond-CyberSecurity-Projects/blob/ed19dec/Images/Django%20Kibana.png)

### Additional Commands Used
| COMMAND |	PURPOSE |
|---------|---------|
| sudo apt-get update | this will update all packages |
| sudo apt install docker.io	| install docker application |
| sudo service docker start	| start the docker application |
| systemctl status docker	| status of the docker application |
| sudo docker pull cyberxsecurity/ansible	| download the docker file |
| sudo docker run -ti cyberxsecurity/ansible bash	| run and create a docker image |
| sudo docker start <image-name>	| starts the image specified |
| sudo docker ps -a	| list all active/inactive containers |
| sudo docker attach <image-name>	| effectively sshing into the ansible |
| ssh-keygen	| create a ssh key |
| ansible -m ping all	| check the connection of ansible containers |

