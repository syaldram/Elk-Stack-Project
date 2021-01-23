## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk Stack DiagramSY](https://user-images.githubusercontent.com/69467182/105615352-df262600-5d9d-11eb-8472-bd877a111da8.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - [/etc/ansible/install-elk.yml](Ansible/Install-elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, improves application responsiveness, also increases availability of
applications and websites for users. In addition to restricting traffic to the network.

- The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks by shifting attack traffic. It does this by shifting attack traffic from the corporate server to a public cloud provider.

- A jump box can give access to the user from a single node that can be secured and monitored. A Jumpbox is a (special-purpose) computer on a network typically used to access devices in a separate security zone. No hardware cost, access, and ease to setup.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics and statistics.

- Filebeat watches for any information in the file system which has been changed and when it has. Filebeat shipper for forwarding and centralizing log data.

- Merticbeat takes the metrics and statistics that it collects and ships them to the output that you specify. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.5   | Linux            |
| Web-2    | Server   | 10.0.0.6   | Linux            |
| ELK      | Server   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- My home IP address (96.228.39.194)

Machines within the network can only be accessed by Local workstation and Jumpbox.

- Jumpbox VM: IP 10.0.0.4 Local Workstation via SSH IP 96.228.39.194

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Home IP              |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK      | No                  | Home IP, 10.0.0.4    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

The main advantage is that you can put commands into multiple servers from a single playbook, like how I combined Filebeat and Metricbeat

The playbook implements the following tasks:

    * Install docker.io
   		- name: Install docker.io
    			apt:
     			update_cache: yes
     			name: docker.io
     			state: present
 	* Install Python-pip
 		- name: Install pip3
    			apt:
     			force_apt_get: yes
     			name: python3-pip
     			state: present
    * Install: docker
   		- name: Install Docker python module
    			pip:
     			name: docker
     			state: present
    * Command: sysctl -w vm.max_map_count=262144
 	- Launch docker container: elk
   		- name: download and launch a docker elk container
    			docker_container:
     			name: elk
     			image: sebp/elk:761
     			state: started
     			restart_policy: always
     			published_ports:
      			- 5601:5601
      			- 9200:9200
      			- 5044:5044

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Elk Instance](https://user-images.githubusercontent.com/69467182/105616149-be140400-5da2-11eb-9b75-9422867cb4f4.JPG)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.5   
  Web-2 10.0.0.6

We have installed the following Beats on these machines:

- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat collects the changes done

![FIlebeat2](https://user-images.githubusercontent.com/69467182/105616235-835e9b80-5da3-11eb-96e8-88b639691427.JPG)

- Metricbeat collects metrics and statistics

![Metricbeat](https://user-images.githubusercontent.com/69467182/105616242-9cffe300-5da3-11eb-857e-36f3501c6c04.JPG)

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible/.
- Update the host file to include...
- Run the playbook, and navigate to command line to check that the installation worked as expected.

  Playbook: install-elk.yml 
    Location: /etc/ansible/install-elk.yml

    Edit the /etc/ansible/host file to add webserver/elkserver ip addresses

    
-    /etc/ansible/hosts
    
    [webservers]
      
    10.0.0.5 ansible_python_interpreter=/usr/bin/python3
    
    10.0.0.6 ansible_python_interpreter=/usr/bin/python3

    [elk]
    
    10.1.0.4 ansible_python_interpreter=/usr/bin/python3


- The URL you navigate to in order to check that the ELK server is running.

-   Use the public IP address of your ELK VM - http://[your.ELK-VM.External.IP]:5601/app/kibana.
    	
    http://20.55.192.45:5601/app/kibana



