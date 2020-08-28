# ANNEX-A
Following are the screens displaying the configuration of each device used on the project. 
All the devices enlisted next are containing in a group resource called RedTeam.
The order listed is the same as they were generated on Azure.

## cyberNet: Virtual Network Configuration:

Main data:
![cyberNet](https://user-images.githubusercontent.com/64491311/91500211-0af5bc00-e891-11ea-950d-fe251a498c36.png)

Add subnet called SCyberNet
![SCyberNet](https://user-images.githubusercontent.com/64491311/91500220-0fba7000-e891-11ea-8126-23b97de4e7e2.png)

Use Azure DNS

![DNS Server](https://user-images.githubusercontent.com/64491311/91500227-13e68d80-e891-11ea-8863-48ded9f0d608.png)

Make sure to peer this network to ELK network
![CyberNet Peering](https://user-images.githubusercontent.com/64491311/91500234-16e17e00-e891-11ea-90b6-077b700634cd.png)

## Firewall RedTeam-NSG:

![Inbound port Rules](https://user-images.githubusercontent.com/64491311/91501277-498c7600-e893-11ea-898b-61c113dbd523.png)

## Jump box Machine configuration:
    
 
![JumpBox](https://user-images.githubusercontent.com/64491311/91498502-de8c7080-e88d-11ea-8585-1dba94dbcbdc.png)

## WEB1&2:

Main data
![WEB12](https://user-images.githubusercontent.com/64491311/91500882-61afc580-e892-11ea-85d4-366969ca01f5.png)



Availability set

![image](https://user-images.githubusercontent.com/64491311/91514926-fbd53500-e8b5-11ea-99af-e1c05dc7b656.png)

## Vnet-ELK: Virtual Network Configuration for ELK Server:

![vnet-elk](https://user-images.githubusercontent.com/64491311/91515711-c2052e00-e8b7-11ea-8868-dfaba30d7363.png)

## ELK Server:
![elk-server](https://user-images.githubusercontent.com/64491311/91515684-b0238b00-e8b7-11ea-8be9-5a5e48009276.png)

ELK server inbound port rules
![ELK-Server-firewall](https://user-images.githubusercontent.com/64491311/91515705-bd407a00-e8b7-11ea-9b96-54ad13dcc513.png)


Note: ELK Server and Web1&2 Machines should contain same existing public key.

