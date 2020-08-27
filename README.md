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
Following table displays the general configuration of each machine.

Following pictures display the configuration of each machine on the lab
Jump Box




