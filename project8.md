## PROJECT 8: Load Balancer Solution With Apache

### **STEP 1 — CONFIGURE APACHE AS A LOAD BALANCER**

1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb

2. Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.

![](./images/tcp_port_80.PNG)


3. Install Apache Load Balancer on **Project-8-apache-lb** server and configure it to point traffic coming to LB to both Web Servers, by running the commands below:

 #Install apache2

`sudo apt update`
`sudo apt install apache2 -y`
`sudo apt-get install libxml2-de`


#Enable following modules:

`sudo a2enmod rewrite`

`sudo a2enmod proxy`

`sudo a2enmod proxy_balancer`

`sudo a2enmod proxy_http`

`sudo a2enmod headers`

`sudo a2enmod lbmethod_bytraffic`

#Restart apache2 service

`sudo systemctl restart apache2`

#Make sure apache2 is up and running

`sudo systemctl status apache2`


![](./images/apache_running.PNG)

Configure load balancing, run the command below:

`sudo vi /etc/apache2/sites-available/000-default.conf`

    #Add this configuration into this section <VirtualHost *:80>  </VirtualHost>

![](./images/config.PNG)



4. Verify that our configuration works – try to access your Load Balancer’s public IP address or Public DNS name from your browser:

    *http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name/index.php*

    ![](./images/load_balancer.PNG)


Note: If in the Project-7 you mounted /var/log/httpd/ from your Web Servers to the NFS server – unmount them and make sure that each Web Server has its own log directory. This is achieved by running the command belwo:



From each Web Servers instance, run following command:

`sudo tail -f /var/log/httpd/access_log`

![](./images/web1.PNG)


![](./images/web2.PNG)



5. Configure Local DNS Names Resolution


    #Open this file on your LB server



`sudo vi /etc/hosts`

    #Add 2 records into this file with Local IP address and arbitrary name for both of your Web Server

    <WebServer1-Private-IP-Address> Web1

    <WebServer2-Private-IP-Address> Web2


![](./images/dns_rsolution.PNG)


Now you can update your LB config file with those names instead of IP addresses. 

Run the command below again:

`sudo vi /etc/apache2/sites-available/000-default.conf`


![](./images/resolved_dns_name.PNG)

You can try to curl your Web Servers from LB locally `curl http://Web1` or `curl http://Web2`


Remember, this is only internal configuration and it is also local to your LB server, these names will neither be ‘resolvable’ from other servers internally nor from the Internet.




## This is the end of Project 8