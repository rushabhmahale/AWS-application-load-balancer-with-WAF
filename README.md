# AWS-application-load-balancer-with-WAF

## Why loadbalacer is necessary
Elastic Load Balancing automatically distributes your incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in one or more Availability Zones. It monitors the health of its registered targets, and routes traffic only to the healthy targets. Elastic Load Balancing scales your load balancer as your incoming traffic changes over time. It can automatically scale to the vast majority of workloads. <br>
![FastSilverJay-max-1mb](https://user-images.githubusercontent.com/63963025/169843771-d86cfb8f-56aa-41e4-988e-4b7932e6c863.gif)

Reffer this doc:- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
## Types of loadbalancer in AWS
- Application Load Balancer
- Network Load Balancer
- Classic Load Balancer
- Gateway Load Balancers <br>
Reffer this doc:- https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-types.html
## Application Load Balancer:- <br>
An Application Load Balancer makes routing decisions at the application layer (HTTP/HTTPS), supports path-based routing, and can route requests to one or more ports on each container instance in your cluster. Application Load Balancers support dynamic host port mapping. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance (such as 32768 to 61000 on the latest Amazon ECS-optimized AMI). When the task is launched, the NGINX container is registered with the Application Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance. <br>
Reffer this doc:- https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html

##  Network Load Balancer:- 
A Network Load Balancer makes routing decisions at the transport layer (TCP/SSL). It can handle millions of requests per second. After the load balancer receives a connection, it selects a target from the target group for the default rule using a flow hash routing algorithm. It attempts to open a TCP connection to the selected target on the port specified in the listener configuration. It forwards the request without modifying the headers. Network Load Balancers support dynamic host port mapping. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance (such as 32768 to 61000 on the latest Amazon ECS-optimized AMI). When the task is launched, the NGINX container is registered with the Network Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance. <br>
Reffer this doc:- https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html

## Classic Load Balancer:- 
A Classic Load Balancer makes routing decisions at either the transport layer (TCP/SSL) or the application layer (HTTP/HTTPS). Classic Load Balancers currently require a fixed relationship between the load balancer port and the container instance port. For example, it is possible to map the load balancer port 80 to the container instance port 3030 and the load balancer port 4040 to the container instance port 4040. However, it is not possible to map the load balancer port 80 to port 3030 on one container instance and port 4040 on another container instance. <br>
Reffer this doc:- https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/introduction.html

## Gateway Load Balancers:-
Gateway Load Balancers allow you to deploy, scale, and manage virtual appliances, such as firewalls, intrusion detection and prevention systems, and deep packet inspection systems. It combines a transparent network gateway (that is, a single entry and exit point for all traffic) and distributes traffic while scaling your virtual appliances with the demand. A Gateway Load Balancer operates at the third layer of the Open Systems Interconnection (OSI) model, the network layer. It listens for all IP packets across all ports and forwards traffic to the target group that's specified in the listener rule. It maintains stickiness of flows to a specific target appliance using 5-tuple (for TCP/UDP flows) or 3-tuple (for non-TCP/UDP flows). The Gateway Load Balancer and its registered virtual appliance instances exchange application traffic using the GENEVE protocol on port 6081. It supports a maximum transmission unit (MTU) size of 8500 bytes. Gateway Load Balancers use Gateway Load Balancer endpoints to securely exchange traffic across VPC boundaries. A Gateway Load Balancer endpoint is a VPC endpoint that provides private connectivity between virtual appliances in the service provider VPC and application servers in the service consumer VPC. <br>
Reffer this doc:- https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/introduction.html

# Steps to be followed :- 

## Step1 Create a Target Groups
Go to EC2 Scroll down there option called Target Group
![image](https://user-images.githubusercontent.com/63963025/169863345-3c07387e-466d-4e68-8ec0-0f1b50a82993.png)
- Create target group
![image](https://user-images.githubusercontent.com/63963025/169863484-ba88af11-a458-4bd1-9b7f-75ce25ef2a74.png)
- Fill the details <br>
- <b>Target group name</b>: Target-ec2
- <b>Protocol</b>: HTTP
- <b>Port</b>: 80
- <b>VPC</b>: default
- <b>Protocol version</b>: HTTP1
![image](https://user-images.githubusercontent.com/63963025/169865233-d87d9ff4-c609-4449-8c68-cd24a0469bf0.png)
- <b>Health Checks</b> 
- <b>Health Check protocol</b>: HTTP
- <b>Health Check path</b>: /index.html
![image](https://user-images.githubusercontent.com/63963025/169865823-5210f82d-c627-47cb-90e1-80e96ea07f8a.png)

## Step2 Create a Ec2 instance 
Go to Ec2 console 
![image](https://user-images.githubusercontent.com/63963025/170025426-9279a4e8-9fe3-448a-9465-7dfee0263aeb.png)
- <b>Name</b>:web1
- <b>Application and OS Images (Amazon Machine Image)</b>:Amazon Machine Image (AMI)
![image](https://user-images.githubusercontent.com/63963025/170025728-2367d8b3-5d24-407f-9af0-0c2548ec0569.png)
- <b>Instance type</b>:Instance type t2.micro
- <b>Key pair (login) </b>: create a new key or you can select existing key in my case i am using exsiting key <br>
![image](https://user-images.githubusercontent.com/63963025/170026856-6ed0aaa8-1984-4530-8576-bb18fdaf60c4.png)
- In network setting select <b>Allow HTTPs traffic from the internet</b> & <b>Allow HTTP traffic from the internet</b>  
 ![image](https://user-images.githubusercontent.com/63963025/170035261-cad15e94-2553-49db-bbc4-5a092522833c.png)
- Configure storage
 ![image](https://user-images.githubusercontent.com/63963025/170035861-9608088a-2cd9-4583-af22-077d84373258.png)
- In Advanced details scroll dow n there is option call User data  
![image](https://user-images.githubusercontent.com/63963025/170061615-2c4b86e1-5328-47b7-ab0a-0721f1574cd4.png)
## Similarly Create 3 more Ec2 instance 
- This time change the User data 
![image](https://user-images.githubusercontent.com/63963025/170085452-6a3f7a30-2e94-4e42-8dfc-b25a9707545d.png)
- Create same 3-EC2 instance here change User data  <b>web3</b>
![image](https://user-images.githubusercontent.com/63963025/170086310-6b97df5a-06c6-4efd-80a3-f5e481a9748f.png)
- here we have Created 3 instance
![image](https://user-images.githubusercontent.com/63963025/170087050-38202463-48a7-4ed3-a447-2898b9ddbb5f.png)

## Step3 Create a Application Loadbalancer 
![image](https://user-images.githubusercontent.com/63963025/170063881-02defe7e-6d50-4754-9cb1-3ee3f53a8590.png)
- Create a Loadbalancer Select Application loadbalancer  
![image](https://user-images.githubusercontent.com/63963025/170078120-83fe780f-20ad-48aa-bed7-eeb21fe2ad8c.png)
![image](https://user-images.githubusercontent.com/63963025/170078519-346d233f-ddb6-4300-8c6b-1846917064ec.png)
- <b>Fill the details</b>
- <b>Load balancer name</b>:web-loadbalancer
- <b>Scheme</b>:Internet-facing
- <b>Ip address type</b>: IPv4
![image](https://user-images.githubusercontent.com/63963025/170079352-0c0d5e95-8342-4d7c-9868-7f6e7f4bec1b.png)
- <b>Networking mapping</b>
- <b>VPC</b>: default VPC
- Select all subnet <b> ap-south-1a, ap-south-1b, ap-south-1c</b>
![image](https://user-images.githubusercontent.com/63963025/170080147-5dcb37a6-bb84-443e-b3e4-f04c5238a732.png)
- <b>Security Groups</b>
- <b>Security Groups</b>: default 
![image](https://user-images.githubusercontent.com/63963025/170080281-95e87ef9-e194-4f64-a9ac-0e81b2302a48.png)
- <b> Listener and routing </b>
- <b> HTTP</b>:80
![image](https://user-images.githubusercontent.com/63963025/170081112-3f205a62-2b82-406f-888d-60cc2945bb28.png)
- summary 
![image](https://user-images.githubusercontent.com/63963025/170080992-c9b87e56-e06f-4a39-9e15-3e1a70e52996.png)
- Your loadbalancer is created Successfully 
![image](https://user-images.githubusercontent.com/63963025/170083487-9dca2601-0551-4705-90ca-9e689f612fec.png)

## Attach Instance in Target Group 
![image](https://user-images.githubusercontent.com/63963025/170087571-d8ec863d-ec02-48de-b066-42876bf5398c.png)
- Select Target-ec2--> In targets--> Register targets <b>select all 3 ec2 instance</b> 
![image](https://user-images.githubusercontent.com/63963025/170088460-d25c251d-0278-46dc-9f4e-b0c9e812e3d0.png)
- include as pending below register pending targets 
![image](https://user-images.githubusercontent.com/63963025/170090648-36995356-dc3c-4f8e-9164-48c869fa8e6a.png)

- This will take some time to initialize your instance 
![image](https://user-images.githubusercontent.com/63963025/170091232-a1f7d9be-3cd4-4973-bf1b-81705b11cf8d.png)
 
- Now copy your Loadbalancer DNS name and paste into your browser 
![image](https://user-images.githubusercontent.com/63963025/170093085-a217b25a-663c-4790-a266-6b9c812912de.png)

- Now you will see that your application traffic is been split in different instance 
![Aws-application-loadbalancer](https://user-images.githubusercontent.com/63963025/170094886-48468f34-4606-446c-8026-9eca009c7f16.gif)

## Creating Ipset
- Go to AWS Firewall Manager 
![image](https://user-images.githubusercontent.com/63963025/170096520-4f3bedce-59cf-4841-86ba-9cc8b3c7c1cf.png)

- Go to IP sets--> create ip sets 
 ![image](https://user-images.githubusercontent.com/63963025/170097238-1c90d761-828f-426d-bed9-ff66fd91ea68.png)
- Now go to google.com and search for <b>Whats my ip</b>
![image](https://user-images.githubusercontent.com/63963025/170101635-451a43f3-c5d1-452e-86a3-98c7a9e4254c.png)
- <b>Ip set details</b>
- <b>Ip set name</b>: Demoipset
- <b>Region</b>: ap-south-1 (mumbai)
- <b>Ipv4</b>:<your ip/32>
![image](https://user-images.githubusercontent.com/63963025/170102424-1aefc95a-1245-48fa-88da-f434cb25c110.png)
- Demo ip is been created 
![image](https://user-images.githubusercontent.com/63963025/170102475-2dbe64e3-ae38-4e28-9f2f-f57055eb94cb.png)

## Creating WAF and whitelisting your ip adress 

## What is AWS Firewall Manager:-
AWS Firewall Manager is a security management service which allows you to centrally configure and manage firewall rules across your accounts and applications in AWS Organizations. As new applications are created, Firewall Manager makes it easy to bring new applications and resources into compliance by enforcing a common set of security rules. Now you have a single service to build firewall rules, create security policies, and enforce them in a consistent, hierarchical manner across your entire infrastructure, from a central administrator account.<br>
Refer to this doc:- https://aws.amazon.com/firewall-manager/

- Create web ACL
![image](https://user-images.githubusercontent.com/63963025/170102824-ab11f12d-9478-47e5-8e1d-53eb1773a594.png)
![image](https://user-images.githubusercontent.com/63963025/170103114-b29160a6-6b00-4deb-824f-9812b7d2238a.png)
- Add AWS resource 
![image](https://user-images.githubusercontent.com/63963025/170103301-e588fdff-ca18-468d-a0a9-83757ce36706.png)
![image](https://user-images.githubusercontent.com/63963025/170103318-aba2618c-c0b4-4568-9b40-df7b63601595.png)
- Next <b> Add rules</b>--> Add my own rules and rule groups --> IPset
![image](https://user-images.githubusercontent.com/63963025/170103766-2f1853fa-0da8-4c01-8e5e-523cba57f0e6.png)
![image](https://user-images.githubusercontent.com/63963025/170103777-345426e5-3273-4514-b962-77660f9123f1.png)
- Add rule all default ---> default
![image](https://user-images.githubusercontent.com/63963025/170103960-6d5152e5-472b-4f10-9700-8aa0b5a9ba6e.png)
![image](https://user-images.githubusercontent.com/63963025/170104012-440e39af-c8b9-4212-823f-859749e1ae66.png)
![image](https://user-images.githubusercontent.com/63963025/170104038-2169d3c4-41c3-485a-b3ff-9c9b7e6e99e7.png)
- review and create web ACL
![image](https://user-images.githubusercontent.com/63963025/170104153-d7522e8c-cb38-4bd4-8440-b6256c1f0136.png)

- Web ACL is created 
 ![image](https://user-images.githubusercontent.com/63963025/170104721-7c44f4bc-ca9b-4115-a9f4-18f65cea5774.png)
- Select webACL--> Associated AWS resources --> Add AWS resources 
![image](https://user-images.githubusercontent.com/63963025/170104855-71892c0e-ae4a-45db-90ad-a1307d328476.png)
- Application loadbalancer --> web-loadbalancer
![image](https://user-images.githubusercontent.com/63963025/170105083-91335dc7-7ade-46c3-a466-e18026d79ee5.png)
 
- Go to EC2--> Loadbalancer --> Copy DNS and paste to your browser 
![image](https://user-images.githubusercontent.com/63963025/170105305-2b4b5597-1f10-4f0b-8407-15ab3ae4d0e1.png)

- you should see <b>403 forbidden</b>  Because of <b>WAF</b> 
## What is WAF
AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits and bots that may affect availability, compromise security, or consume excessive resources. AWS WAF gives you control over how traffic reaches your applications by enabling you to create security rules that control bot traffic and block common attack patterns, such as SQL injection or cross-site scripting. 
Reffer to this doc:- https://aws.amazon.com/waf/#:~:text=AWS%20WAF%20is%20a%20web,security%2C%20or%20consume%20excessive%20resources.

![image](https://user-images.githubusercontent.com/63963025/170105477-95505c19-972e-4c71-92d1-67f2748f0aba.png)

- lets use VPN and verrify our WAF is working or not in my case i am using psiphon you can use any VPN (Virtual private network)
## What is VPN 
A VPN hides your IP address by letting the network redirect it through a specially configured remote server run by a VPN host. This means that if you surf online with a VPN, the VPN server becomes the source of your data. This means your Internet Service Provider (ISP) and other third parties cannot see which websites you visit or what data you send and receive online. A VPN works like a filter that turns all your data into "gibberish". Even if someone were to get their hands on your data, it would be useless.<br>
reffer to this doc:- https://www.kaspersky.com/resource-center/definitions/what-is-a-vpn <br>
Download psiphon:- https://psiphon.ca/en/download.html

- VPN is been connected 
![image](https://user-images.githubusercontent.com/63963025/170106200-3152106c-4c79-4fd9-aaac-ef2b8781c5e1.png)

![image](https://user-images.githubusercontent.com/63963025/170106340-2825594c-13c0-41ed-acbd-8fb0cb9c53b8.png)

- Here we are succesfully running your Loadbalancer with using WAF 
![psiphon-with-WAF](https://user-images.githubusercontent.com/63963025/170106987-06b8f4b1-420b-45a0-a639-595cf5ec8c41.gif)

