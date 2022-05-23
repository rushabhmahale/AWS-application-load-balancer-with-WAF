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
- Health Checks 
- <b>Health Check protocol</b>: HTTP
- <b>Health Check path</b>: /
![image](https://user-images.githubusercontent.com/63963025/169865579-bc2c668b-f70e-4a40-a24b-714f8d9055f8.png)

