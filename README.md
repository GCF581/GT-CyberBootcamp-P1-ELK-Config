# GT-CyberB-P1-ElkServer set up
Project 1: Elk server setup using a cloud network on Azure.
The project explains how to set up an Elk server, file beats, and micro beats for a network created under the Azure platform. 
Internal PC's and servers were set up as containers, so this project explains as well the use of docker and ansible-playbooks.

## Project contents
* Network diagram and topology Description
* Machines Configuration
* Access Policies
* Docker and and Ansible configuration
* ELK Configuration
  - Beats in use
  - Machines being monitoring

### Network Diagram and topology Description

![GT_CyberP1_RedTeam_Network_Diagram](https://user-images.githubusercontent.com/64491311/91365496-4760e380-e7cf-11ea-8b53-4214b6659153.png)

The diagram displayed above shows the architecture developed to set up the ELK server and monitor internal webservers (1,2) on DVWA web page (the D*mn Vulnerable Web Application). Those web servers are communicated to a load balancer through SSH communication protocol, while the load balancer is communicated to the web using HTTP. By doing that the PCs 1&2 are kept in the private network and the load balancer is the only one exposed on the public network.

The load balancer is protected by firewall rule which allows external traffic to the virtual network. for this project host PC is the only sourced allowed to access the load balancer. It is important to include a load balancer when using virtualization, as it can defend an organization by denial of service attacks.

A jump box was created to deploy the information of the containers to the other servers. It can be considered as the main repository of the docker. 
Docker is used to create the environment to run different applications. In this project, the docker was generated by pulling an image/template that contained already the required environment to be deployed. (image: frosty_jang).

By using the jump box as the main repository, it helps to develop a system with fewer hardware resources, since there is no necessity to generate more virtual machines which consume system and memory performance on the hosts/clouds, instead, the deployment of the environment and applications is done as containers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network, containers, and system performance.
By using Beats the ELK server is able to collect logs data and metrics of the 2 web-pcs. 
* Filebeat is used to send in a light form all the logs gathered from the Internal PC's. In this way, the system resources used are reduced  than if we were  using logstash
  directly on each machine 
* Metricbeat is used to collect and send to ELK server the statistics and metrics of the operating system, container performance, and services running on the machines. 

### Machines Configuration
Following table displays the general configuration of each machine.  (For safety reasons Public IP'S are not complete displayed)

| Name           | Function             |      IP Address            | Operating System |                             General Notes                                     |
|--------------- |----------------------|----------------------------|------------------|-------------------------------------------------------------------------------|
| Jump Box       | Gateway              | 52.188.XXX.XXX             | Linux            |It contains the connection to docker and deploy containers                     |
| WEB1           | Internal PC          | 10.0.0.5                   | Linux            |Internal PC used as container, it sends logs and syst performance to ELK       |
| WEB2           | Internal PC          | 10.0.0.6                   | Linux            |Internal PC used as container, it sends logs and syst performance to ELK       |
| ELK Server     | ELK Information collector  | 10.1.0.4 / 52.252.XXX.XXX  | Linux      |External IP is used to enter to Kibana to set up Lifebeat and metricbeat and monitor results                                                                                                                                                                 |

Annex-A File Displays the configuration of each device on the network.

### Access Policies

As it is mention and shown on the picture above, Web 1&2 PC's are not exposed to the public internet they belong to a private network (cyberVnet)

Jumpbox and Elk server are the only machines that can be accessed from the Internet, this configuration is set on the virtual machine and inbound ports (Annex-A). 

IP Address for Jump box = Host IP
IP Address for ELK Server = 52.252.XXX.XXX (public address)


| Name       | Publicly Accessible | Allowed IP Addresses       |
|------------|---------------------|----------------------------|
| Jump Box   | Yes                 | 10.0.0.5 10.0.0.6          |
| Elk Server | Yes                 | 10.0.0.5 10.0.0.6 10.0.0.4 |
|            |                     |                            |

### Use of Playbooks and containers development

Ansible was used to deploy the environment and configuration to each container. In that way, all the package installation was performed automatically. 
The configuration and installation of the services of each container were performed by playbooks which are some kind of manifesto indicating software to be installed, ports to be used, etc.
The advantages to use ansible to automate the deployment of the container applications are that it can be run in several machines at the same time, and also the playbook can be modified in case any other requirement is needed. 

**Docker installation steps:**
* Access Jump Box with: `ssh [user]@[jump box Public IP]`
* Change to root user:  `sudo su`
* Install Docker.io:    `apt install docker.io`
* Start docker service: `systemctl start docker`
* Pull the container:   `docker pull [specific container]` (in this case cybersecurity/ansible)
* Connect to docker:    `docker run -ti cybersecurity/ansible bash`
* Set inbound security _rule to the firewall RedTeam-NSG (see Annex-A, Rule1499 VNET22)_

**Docker access steps:**
* Pull existent images from docker: `docker images`
* Review available images:          `docker container list -a`
![container image list](https://user-images.githubusercontent.com/64491311/91520225-c125c980-e8c2-11ea-86c5-80b21f3d3c53.png)

* Start one of the containers:      `docker start [container image]` (this case frosty_jang)
* Attach the image to the container: `docker attach frosty_jang`

following image displays when docker is accessed after attaching the selected image.
![inside docker](https://user-images.githubusercontent.com/64491311/91522201-aace3c80-e8c7-11ea-864e-76b48fce59cd.png)

**Ansible configuration steps:**
* Create new public keys for Elk server, web1&2: `ssh-keygen` (save it on .ssh/id_rsa.pub)
* Copy the key from **id_rsa.pub** to the Elk server, web 1&2 on the Virtual machines. (user must be the same)

![key](https://user-images.githubusercontent.com/64491311/91522213-b28de100-e8c7-11ea-9e6f-2ac7d847d63b.png)

* Config ansible files: `nano /etc/ansible/hosts`
* Uncomment **[webservers]** and add: `[web1 IP] ansible_python_interpreter=/usr/bin/python3` (add another column with WEB2 IP Address)

![Hosts_webservers](https://user-images.githubusercontent.com/64491311/91522306-ecf77e00-e8c7-11ea-962c-649d8208b760.png)

* Add **[ELK]**
* Add `[ELK Server IP] ansible_python_interpreter=/usr/bin/python3`

![hosts_ELK](https://user-images.githubusercontent.com/64491311/91522314-f254c880-e8c7-11ea-8dcc-2bad4a3054b1.png)

* Open ansible config: `nano /etc/ansible/ansible.cfg`
* Uncomment remote user: **remote user = ELK Server, web1&2 user**
* Check communication with the three computers: `ansible -m ping all`

![pingpong](https://user-images.githubusercontent.com/64491311/91522205-af92f080-e8c7-11ea-8741-5982fcec5559.png)

### Elk configuration
Playbook Install-elk.yml was used to deploy all packages and configurations needed to the ELK Server. (for code see Playbook Elk server)

**ELK Playbook execute following steps:**
* Sets the host where the playbook will run (ELK, configured on Ansible.cfg)
* Sets the remote user which will access the ELK Server. (that user must be the same as the one set on the ansible.cfg.
* Update the system
* Installs python3-pip
* Install Docker module 
* Increases memory and sets the use of memory on the container (ELK-server)
* Download the container to the Elk-server
* Open ports 5601, 9200 and 5044 (which are used for the access of the kibana and the transmission of the logs to the server)

**ELK server configuration steps:**

* Run all the commands as the root user.
* Prepare playbook **Install-elk.yml** (see code on Playbook Elk Server)
* Deploy the container to ELK Server by running the playbook: `ansible-playbook install-elk.yml` (make sure to be on ansible directory where the playbook is located)

The following picture displays the success of each step listed on the playbook. 
As it is shown everything was done just on the ELK server machine with IP: 10.1.0.4

![ELK-Server-playOK](https://user-images.githubusercontent.com/64491311/91625357-b6c40800-e974-11ea-9d2d-aa2dcc2033b9.png)

**Beats installation:**
Filebeat and Metricbeat playbooks are prepared to deploy their configuration on web1&2.
But first the configuration files for filebeats and metricbeats are installed on the ELK server.
The configuration files of the beats are on kibana and it is accessed by using the ELK-Server Public IP:  http://[ELK-Server IP]:5601/app/kibana. 

The file to install the image for filebeat refer to: logs/system logs/ DEB
![filebeat-systlogsfile](https://user-images.githubusercontent.com/64491311/91670153-e72cb300-eae8-11ea-930f-a00a18964211.png)

The file to install the image for metricbeat refer to metrics/system metrics/ DEB
![metricbeat-metricsfile](https://user-images.githubusercontent.com/64491311/91670154-e7c54980-eae8-11ea-9ee1-7afbd5698c59.png)


Filebeat-config and metricbeat-configuration serve to set up the modules on the PC’S to be monitoring, to do that is necessary to enable the following features:

* Enable the kibana module as it is shown on the picture below.
![filebeat-kibanamodule](https://user-images.githubusercontent.com/64491311/91669764-98314e80-eae5-11ea-90c8-b16ce921c432.png)

* At elastic search output set the host [10.1.0.4:9200] (ELK server private IP: Port 9200, which allows the traffic of the logs)
![filebeat-elasticsearch](https://user-images.githubusercontent.com/64491311/91669765-98c9e500-eae5-11ea-88ca-817b07871043.png)

* Setup kibana  as host 10.1.0.4:5601 (ELL Server private IP:Port5601 which allows the traffic to kibana of the elk server
![filebeat-kibana5601](https://user-images.githubusercontent.com/64491311/91669766-98c9e500-eae5-11ea-8153-a8bcc028f6b9.png)


**Filebeat playbook (filebeat-playbook.yml)  includes:**
* Loading configuration on webservers (web1&2)
* Loading and installing files for Filebeat Configuration
* Configuring the filebeat modules
* Set up of the filebeat.


**Metricbeat playbook (metricbeat-playbook.yml) includes:**

* Loading configuration on webservers (web1&2)
* Loading and installing files for Filebeat Configuration
* Configures and enable modules for metric beat
* Set up and start metric beat service








