


# Misc

## Network / DNS Archy

- EC2 can have multiple Elastic Network Interfaces, however all should be in same AZ
- Each NI has 
    - mac
    - 1 primary private IP4 .. static .. donot change for life time of instance .. dns (ip.ec2.internal .. only resolve inside private region)
    - 0 or more secondary IP's
    - 0 or 1 public IP4 - changes after start/stop (restart won't stop) - (ec2-ip.compute-1.. -> redirects to private ip inside vpc else public ip)
    - 1 elastic IP per private IPv4 - attached to instance (when attached, non elastic public ip will be removed)  
    - 0 or more ip6 (all are publicly routable)
    - security groups attached to interface
    - source/destination check
    - secondary ni can be detachable

- Secondary ENI + MAC = Licensing -> can we moved between instances by attaching/detaching ni
- Multiple interfaces vs Multiple IPs
    - Different security groups for multiple interfaces
- OS - Doesn't see public IPv4
- Normal public IP4 are Dynamic 
- public DNS = private IP in VPC, public IP everywhere else

## Demo - Wordpress on EC2

- 