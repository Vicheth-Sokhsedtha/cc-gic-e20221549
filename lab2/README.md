# Cloud Lab 2 — Application Load Balancer and Auto Scaling

## Description

This lab demonstrates how an **Application Load Balancer (ALB)** distributes HTTP requests across EC2 instances managed by an **Auto Scaling group (ASG)**. A target group tracks the instances and checks whether their web applications are healthy. If an instance fails its health checks, the ASG replaces it.

The setup uses the default VPC in **US East (N. Virginia) (`us-east-1`)**:

- `LoadBalancerSecurityGroup` allows inbound HTTP traffic to the load balancer.
- `LaunchTemplateSecurityGroup` allows inbound HTTP and SSH traffic to the EC2 instances.
- `LoadBalancerTargetGroup` routes HTTP traffic to the instances and performs health checks.
- `MyLoadBalancer` is an internet-facing ALB listening on HTTP port 80.
- `AutoScalingLaunchTemplate` launches Amazon Linux 2023 `t2.micro` instances and starts an Apache (`httpd`) web server.
- `MyAutoScalingGroup` keeps two instances running, with a maximum capacity of three, and is attached to the target group.

After both targets become healthy, opening the load balancer's DNS name displays a message containing the hostname of the instance that handled the request. Refreshing the page demonstrates traffic being served by different instances. Stopping `httpd` on one instance causes its health check to fail; the ASG then launches a replacement instance.

## Screenshots

### Security groups and target group

1. **Load balancer security group** — HTTP inbound rule for `LoadBalancerSecurityGroup`.  
   ![Load balancer security group](images/01-sg-load-balancer.png)
2. **Launch template security group** — HTTP and SSH inbound rules for `LaunchTemplateSecurityGroup`.  
   ![Launch template security group](images/02-sg-launch-template.png)
3. **Target group** — `LoadBalancerTargetGroup` configuration.  
   ![Target group details](images/03-target-group.png)
4. **Health checks** — target group health check settings.  
   ![Target group health checks](images/04-health-checks.png)

### Load balancer and Auto Scaling group

5. **Load balancer list** — `MyLoadBalancer` in the AWS console.  
   ![Load balancer list](images/05-load-balancer-list.png)
6. **Load balancer details** — internet-facing scheme and security group.  
   ![Load balancer details](images/06-load-balancer-details.png)
7. **Launch template** — instance image, type, security group, and user data.  
   ![Launch template details](images/07-launch-template.png)
8. **Auto Scaling group list** — `MyAutoScalingGroup` in the AWS console.  
   ![Auto Scaling group list](images/08-asg-list.png)
9. **Auto Scaling group details** — capacity settings and attached target group.  
   ![Auto Scaling group details](images/09-asg-details.png)

### Testing load balancing and recovery

10. **First browser request** — response from an instance through the load balancer.  
    ![First load balancer response](images/10-browser-1.png)
11. **Second browser request** — response showing a different instance hostname.  
    ![Second load balancer response](images/11-browser-2.png)
12. **Application failure** — terminal after stopping the Apache web server.  
    ![Stopped web server](images/12-stop-httpd.png)
13. **Unhealthy target** — target group showing the failed instance as unhealthy or draining.  
    ![Unhealthy target](images/13-target-unhealthy.png)
14. **Replacement target** — target group showing the replacement instance as healthy.  
    ![Healthy replacement target](images/14-target-replaced.png)