Project1

Enrique Espinoza 
06/4/2021




 Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1ekRzoac9cm-IdjdiD0nQ74CojC17Y69Y/view?usp=sharing


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the file may be used to install only certain pieces of it, such as Filebeat.

 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


 Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secure, in addition to restricting network traffic to the network.

What aspect of security do load balancers protect? They protect by shifting the DDOS attack traffic from the corporate server to a public cloud server, thus allowing balanced traffic not to harm or pause the site.

What is the advantage of a jump box? It's a secure first step in logging in a controlled and secured environment.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

What does Filebeat watch for? It monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing. 

What does Metricbeat record? It helps you monitor your servers by collecting metrics from the system and services running on the server such as Apache. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux       |
| Web169    | Gateway  | 10.0.0.7   | Linux       |
| Web269    | Gateway  | 10.0.0.8   | Linux       |
| Elkvm        | Gateway   | 10.1.0.4  |  Linux      |

 Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

Add whitelisted IP addresses: 
24.4.247.57

Machines within the network can only be accessed by the Jumpbox.

 Which machine did you allow to access your ELK VM? What was its IP address? 
Jumpbox and the IP address is 20.98.77.39 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
| Elkvm    |     no              | 20.98.77.39, 24.4.247.57 |
|          |                     |                      |

 Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible? The primary benefit of Ansible is it allows IT administrators to automate away the drudgery from their daily tasks.


The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc
By making a yml playbook and adding the commands to install dockers.
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
- name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes





The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://drive.google.com/file/d/1kjLFbB1ZlSaFqOfLTXzvALl7QSnTgbHJ/view?usp=sharing 

Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web169 and Web269 and the Elkvm

List the IP addresses of the machines you are monitoring:
Web169 10.0.0.7, Web269 10.0.0.8, and Elkvm 10.1.0.4

We have installed the following Beats on these machines:

Specify which Beats you successfully installed: we installed both Filebeat and Metricbeat.

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.

Filebeat: will collect log data and send it to the specified area that was made in yml txt and an example would be collecting logs of access and sending them to a allocated area

Metricbeat: collects statistics and then sends them to an allocated area specified by the user same as filebeat.


Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
 Copy the configure and playbook  file to the ansible directory and then using an editor
 Update the configuration file to include the location and installation of filebeat and metricbeat
 Run the playbook, and navigate to kibana and select the systems logs or metric logs and verify the data to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:
 Which file is the playbook? Where do you copy it? 

---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start

    # Use systemd module
  - name: Enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes

That is the Filebeat playbook and its copied to the ansible directory where it is edited to match the user location and the updated version found in the Kibana site that is accessed through the Elkvm.




Which file do you update to make Ansible run the playbook on a specific machine? How do I                   specify which machine to install the ELK server on versus which to install Filebeat on? You update the configuration file and then make sure the playbook is set to aim at that configuration file that was edited, by installing filebeat on the server you need it on and then telling it where to log that data.

 Which URL do you navigate to in order to check that the ELK server is running?
You type in the Elk Public ip address in which case is http://13.84.212.163:5601 this will send your web browser to a site called Kibana.
