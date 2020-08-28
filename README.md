# GT-CyberB-P1-ElkServer set up
Project 1: Elk server setup using a cloud network on Azure.
The project explains how to set up an Elk server, file beats, and micro beats for a network created under the Azure platform. 
Internal PC's and servers were set up as containers, so this project explains as well the use of docker and ansible-playbooks.

## Project contents
* Network diagram and topology Description
* Machines Configuration
* Access Policies
* Use of Playbooks and containers development
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



![container image list](https://user-images.githubusercontent.com/64491311/91520225-c125c980-e8c2-11ea-86c5-80b21f3d3c53.png)

![inside docker](https://user-images.githubusercontent.com/64491311/91520577-9d16b800-e8c3-11ea-9234-bca637be0581.png)

![key](https://user-images.githubusercontent.com/64491311/91520891-760cb600-e8c4-11ea-8e08-25d6929e2ead.png)

