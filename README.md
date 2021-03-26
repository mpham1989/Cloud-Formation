
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Alt text](https://raw.githubusercontent.com/mpham1989/Marty-Pham/main/Diagrams/Untitled%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on AWS. They can be used to either recreate the entire deployment pictured above. 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting incoming traffic to the network.
- Appliance-basedLoad-Balancers will help protect against threats such as SQL injection and Cross-Site scripting. 
What is the advantage of a jump box? - Having a jumpbox provides a high level of security by making databases and webservers inaccesible to the public. It can only be accessed by a using a unique key through an SSH TCP protocol.  
The Jumpbox will also house and run the needed the containers and images to allow the webservers and databases to communicate.
 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat monitors log files/locations offering a simple and light way to centralize log events. 
- Once deploying metricbeat it will collect the metrics and statistics from your systms and services from operating system, CPU, memory running on the server. 

The configuration details of each machine may be found below.

| Name           | Function    | IP Address | Operating System    |
|----------      |----------   |------------|------------------   |
| Jumpbox/ansible| Gateway     | 10.10.0.20 | Linux (ec2)         |
| DVWA1          | Webserver   | 10.10.2.122| Linux (ubuntu 20.04)|
| DVWA2          | Webserver   | 10.10.2.90 | Linux (ubuntu 20.04)|
| ELK            | Event Logger| 10.10.2.63 | Linux (ubuntu 20.04)|
| RDP            | Viewer      | 10.10.0.29 | Windows Server 2019 | 

### Access Policies            

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine and RDP can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
```10.10.2.122, 10.10.2.90, 10.10.2.63```

Machines within the network can only be accessed by SSH.
- The only machines that can access the ELK server is the jumpbox. ```10.10.0.20```

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses   |
|----------|---------------------|----------------------  |
| Jump Box | No                  | 10.0.0.1 10.0.0.2      |
| DVWA1    | No                  | 10.10.0.20             |
| DVWA2    | No                  | 10.10.0.20             |
| ELK      | No                  | 10.10.0.20             |
| RDP      | No                  | 10.10.0.20             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automating this practice saves time and it has been tested, therefore it will have no mistakes once the playbook has been deployed.

The playbook implements the following tasks:
- Download pre-configured install-elk.yml file and transfer the file from the local folder to jumpbox using the following command: 
- ```scp -i "Virginakey.pem" install-elk.yml ec2-user@ec2-54-89-238-20.compute-1.amazonaws.com/home/ec2-user```
- SSH into jumpbox and check docker status and start docker service using command: 
 ```sudo service docker status> <sudo service docker start```

- navigate into root by using following command: ```Sudo docker run -t -I cyberxsecurity/ansible bash```
- Open a separate gitbash or cmd terminal and locate the container, followed by transferring the install-elk.yml file to the root. 
- ```sudo docker ps``` ```sudo docker cp install-elk.yml (containerid):/root```
![Alt text](https://raw.githubusercontent.com/mpham1989/Cloud-Formation/main/images/Copying%20key.png)

- on the root window, navigate into the hosts file and add the [elk] header with IP: ```10.10.2.63``` underneath the webservers using following command: 
cd /etc/hosts 

[Webservers]

```10.10.2.122```

```10.10.2.90```

[elk]

```10.10.2.63```


-SSH into elk server, update and upgrade the unbuntu machine followed by installing

```ansible-playbook install-elk.yaml --key-file Virginakey.pem```


The following screenshot displays the result of the elk server after successfully configuring the ELK instance.

![Alt text](https://raw.githubusercontent.com/mpham1989/Marty-Pham/main/images/Kibana.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
 ```[10.10.2.122, 10.10.2.90]```

We have installed the following Beats on these machines:
- Filebeat

This Beat allows us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. It helps you keep the simple things simple by offering a lightweight way to forward and centralize logs and files. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat.yml and filebeat-playbook.yml file to ansible:/root .
- Update the filebeat.yml configuration file to include elk-server ip to the elasticsearch and kibana category in the configuration file.
- Run the playbook, and navigate to elk-server to check that the installation worked as expected.

- The filebeat-playbook.yml is the playbook file to install or deploy that needs to be copied to " /etc/ansible/hosts/" directory.
- Which file do you update to make Ansible run the playbook on a specific machine? -filebeat.yml which is the configuration file which will deploy into the elk-server. How do I specify which machine to install the ELK server on versus which to install Filebeat on? - you would edit the hosts file and create a new header called elkservers and add the private ip of the elkserver instance. Then configure the filebeat.yml file to add the private ip of the elk-server into two lines within the file. Lines 1106 and 1806 which are the hosts ips.  
- Once filebeat is deployed, please use the following URL to verify that it's working ```10.10.2.63:5601```
![Alt text](https://raw.githubusercontent.com/mpham1989/Cloud-Formation/main/images/Filebeat.png)

Bonus

Step 1: Open Cloud formation

Step 2: Use the cloud formation template to deploy your network
Step 3: Create a Linux 2 ec2 instance in your public subnet. (This instance will be hosting the ansible container)
Step 4: Copy the public to your instance
scp -i "<key name>" <key name> ec2-user@ec2-##-###-###-##.us-region.compute.amazonaws.com:/home/ec2-user
![Alt text](https://raw.githubusercontent.com/mpham1989/ELK-Stack-amd-Filebeat-Project/main/images/copying%20virigina%20key.png)
Step 5: Connect to your ec2 instance using ssh
ssh -i "<key name>" ec2-user@ec2-##-###-###-##.us-region.compute.amazonaws.com
Step 6: install docker
Sudo yum install docker -y
![Alt text](https://raw.githubusercontent.com/mpham1989/ELK-Stack-amd-Filebeat-Project/main/images/isntall%20docker.png)
Step 7: Create a daemon.json file in the /etc/docker path
Sudo nano /etc/docker/daemon.json
Copy the following to the file to the daemon file
{
 "default-address-pools":
 [
 {"base":"10.10.0.0/24","size":24}
 ]
}
Step 8: Start the docker service
Sudo service docker start
Check the status of docker service
Sudo service docker status
  ![Alt text](https://raw.githubusercontent.com/mpham1989/Cloud-Formation/main/images/docker%20status.png)
Step 9: Pull the ansible image using the following command
Step 10: Sudo docker pull cyberxsecurity/ansible
Step 11: Run the ansible container
Step 12: Sudo docker run -ti cyberxsecurity/ansible bash
Step 13: Open a second cmd connection to your ec2 instance
Run the following command to get your container ID
Sudo docker ps
Step 14: Copy the key to your ansible container
Sudo docker cp <key> <container id>:/root
![Alt text]( https://raw.githubusercontent.com/mpham1989/ELK-Stack-amd-Filebeat-Project/main/images/sudo%20docker%20ps.png)
Move to your second cmd and type “ls”. The key should be visible if copied correctly

Step 15: Create DVWA ec2 instances

In the ec2 dashboard select an Ubuntu image. 
Select the new VPC named VPC1
Select the private subnet to install Ubuntu
Make sure you are creating 2 instances



From your ansible container ping the IP addresses of the Ubuntu machine
From your ansible container, ssh into each machine 
Ssh -i <keyname> ubuntu@<IPaddress>

Configure ansible container:
Cd etc/ansible
Ls and look for hosts and .cfg file
First nano hosts file
Uncomment webservers in hosts file

Use private IP address of private machine
Ansible will try connecting to this machine
Now nano ansible.cfg file

Uncomment remote_user and set value equal to “ubuntu” (without quotes)

Attempt to ping all using the following
ansible -m ping all -–key-file <key name>
Should fail (our subnet is different from our targets)
Should be green on success
Create ansible_config.yaml using the following code for Ubuntu in the same path as your key

---
  - name: Config Web VM with Docker
    hosts: webservers
    become: true
    tasks:
    - name: docker.io
      apt:
        name: docker.io
        state: present
  
    - name: Install pip
      apt:
        name: python3-pip
        state: present
  
    - name: Install Docker python module
      pip:
        name: docker
        state: present
  
    - name: download and launch a docker web container
      docker_container:
        name: dvwa
        image: cyberxsecurity/dvwa
        state: started
        published_ports: 80:80

If ping is successful, install playbook:
ansible-playbook ansible_config.yaml --key-file=Key1.pem
If ping still not working, try sshing into one of the ubuntu machines first, then exit then ping again (don’t know why this works lul)


Create a load balancer now
Load balancer is under ec2
Create application load balancer
Give any name
Make sure its internet facing
IPv4 will be used
Select our VPC1
Select availability zones to be our Private and Public subents
Connect to the dvwa instance website
Connect via ec2 instance public address.
172.17.0.2
10.10.0.254
