# awscli_cmd
CLI commands for AWS 
#--------------------------------------------------------------------------------#
STEP-1: Create a VPC
# aws ec2 create-vpc --cidr-block 10.0.0.0/16

{
    "Vpc": {
        "VpcId": "vpc-02c6f6694acc5b388",
        "InstanceTenancy": "default",
        "Tags": [],
        "CidrBlockAssociationSet": [
            {
                "AssociationId": "vpc-cidr-assoc-0c4b15815d7c56c4e",
                "CidrBlock": "10.0.0.0/16",
                "CidrBlockState": {
                    "State": "associated"
                }
            }
        ],
        "Ipv6CidrBlockAssociationSet": [],
        "State": "pending",
        "DhcpOptionsId": "dopt-a43781dc",
        "CidrBlock": "10.0.0.0/16",
        "IsDefault": false
    }
}

STEP-2: Create a IGW & Attach to VPC
# aws ec2 create-internet-gateway

{
    "InternetGateway": {
        "Tags": [],
        "Attachments": [],
        "InternetGatewayId": "igw-0359b9156112ee2a0"
    }
}

# aws ec2 attach-internet-gateway --vpc-id "vpc-02c6f6694acc5b388" --internet-gateway-id "igw-0359b9156112ee2a0"

STEP-3: Create 2 Route Tables i.e. 1. Public & 2. Private route table and attach Public RouteTable with IGW
# aws ec2 create-route-table --vpc-id "vpc-02c6f6694acc5b388"

{
    "RouteTable": {
        "Associations": [],
        "RouteTableId": "rtb-0f7b357d655c1ac17",
        "VpcId": "vpc-02c6f6694acc5b388",
        "PropagatingVgws": [],
        "Tags": [],
        "Routes": [
            {
                "GatewayId": "local",
                "DestinationCidrBlock": "10.0.0.0/16",
                "State": "active",
                "Origin": "CreateRouteTable"
            }
        ]
    }
}

# aws ec2 create-route-table --vpc-id "vpc-02c6f6694acc5b388"

{
    "RouteTable": {
        "Associations": [],
        "RouteTableId": "rtb-0ddc10ca4812d56c9",
        "VpcId": "vpc-02c6f6694acc5b388",
        "PropagatingVgws": [],
        "Tags": [],
        "Routes": [
            {
                "GatewayId": "local",
                "DestinationCidrBlock": "10.0.0.0/16",
                "State": "active",
                "Origin": "CreateRouteTable"
            }
        ]
    }
}

Create a route in the route table that points all traffic (0.0.0.0/0) to the Internet gateway.
# aws ec2 create-route --route-table-id "rtb-0f7b357d655c1ac17" --destination-cidr-block 0.0.0.0/0 --gateway-id "igw-0359b9156112ee2a0"
{
    "Return": true
}

STEP-4: Create 6 Subnets i.e. 2 Subnets will be Public & 4 should be Private part of 2 Different Availability Zones
# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.1.0/24 --availability-zone "us-east-1a"
{
    "Subnet": {
        "AvailabilityZone": "us-east-1a",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-091331be378b2807a",
        "CidrBlock": "10.0.1.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}

Enable Auto-Assign-Public-IP to Subnet :
# aws ec2 modify-subnet-attribute --subnet-id "subnet-091331be378b2807a" --map-public-ip-on-launch

# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.2.0/24 --availability-zone "us-east-1b"

{
    "Subnet": {
        "AvailabilityZone": "us-east-1b",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-02201f4ce3f502164",
        "CidrBlock": "10.0.2.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}

Enable Auto-Assign-Public-IP to Subnet :
# aws ec2 modify-subnet-attribute --subnet-id "subnet-02201f4ce3f502164" --map-public-ip-on-launch


# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.3.0/24 --availability-zone "us-east-1a"

{
    "Subnet": {
        "AvailabilityZone": "us-east-1a",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-028199674d5e18d19",
        "CidrBlock": "10.0.3.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}


# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.4.0/24 --availability-zone "us-east-1b"

{
    "Subnet": {
        "AvailabilityZone": "us-east-1b",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-0be15c2bf5700ef89",
        "CidrBlock": "10.0.4.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}

# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.5.0/24 --availability-zone "us-east-1a"
{
    "Subnet": {
        "AvailabilityZone": "us-east-1a",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-084e774a2a5354d3c",
        "CidrBlock": "10.0.5.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}

# aws ec2 create-subnet --vpc-id "vpc-02c6f6694acc5b388" --cidr-block 10.0.6.0/24 --availability-zone "us-east-1b"

{
    "Subnet": {
        "AvailabilityZone": "us-east-1b",
        "AvailableIpAddressCount": 251,
        "DefaultForAz": false,
        "Ipv6CidrBlockAssociationSet": [],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "MapPublicIpOnLaunch": false,
        "SubnetId": "subnet-08ef3e2e2885cfd1f",
        "CidrBlock": "10.0.6.0/24",
        "AssignIpv6AddressOnCreation": false
    }
}


# aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-02c6f6694acc5b388" --query 'Subnets[*].{ID:SubnetId,CIDR:CidrBlock}'

[
    {
        "CIDR": "10.0.2.0/24",
        "ID": "subnet-02201f4ce3f502164"
    },
    {
        "CIDR": "10.0.6.0/24",
        "ID": "subnet-08ef3e2e2885cfd1f"
    },
    {
        "CIDR": "10.0.3.0/24",
        "ID": "subnet-028199674d5e18d19"
    },
    {
        "CIDR": "10.0.5.0/24",
        "ID": "subnet-084e774a2a5354d3c"
    },
    {
        "CIDR": "10.0.4.0/24",
        "ID": "subnet-0be15c2bf5700ef89"
    },
    {
        "CIDR": "10.0.1.0/24",
        "ID": "subnet-091331be378b2807a"
    }
]

STEP-5: 2 Public Subnet Association with Public Route Table & 4 Private Subnets association with Private RouteTable
# aws ec2 associate-route-table  --subnet-id "subnet-091331be378b2807a" --route-table-id "rtb-0f7b357d655c1ac17"
{
    "AssociationId": "rtbassoc-0c43f045fcbf8368c"
}

# aws ec2 associate-route-table  --subnet-id "subnet-02201f4ce3f502164" --route-table-id "rtb-0f7b357d655c1ac17"
{
    "AssociationId": "rtbassoc-0f3771be13ab2d9e2"
}

# aws ec2 associate-route-table  --subnet-id "subnet-028199674d5e18d19" --route-table-id "rtb-0ddc10ca4812d56c9"
{
    "AssociationId": "rtbassoc-07375235867165543"
}
# aws ec2 associate-route-table  --subnet-id "subnet-0be15c2bf5700ef89" --route-table-id "rtb-0ddc10ca4812d56c9"
{
    "AssociationId": "rtbassoc-058cf12d2517bb7d5"
}
# aws ec2 associate-route-table  --subnet-id "subnet-084e774a2a5354d3c" --route-table-id "rtb-0ddc10ca4812d56c9"
{
    "AssociationId": "rtbassoc-0f1375226415ff631"
}
# aws ec2 associate-route-table  --subnet-id "subnet-08ef3e2e2885cfd1f" --route-table-id "rtb-0ddc10ca4812d56c9"
{
    "AssociationId": "rtbassoc-08018c339f30f36c5"
}

STEP-5.A : To create a network ACL for the specified VPC
# aws ec2 create-network-acl --vpc-id "vpc-02c6f6694acc5b388"

{
    "NetworkAcl": {
        "Associations": [],
        "NetworkAclId": "acl-0254e40151a20d0ed",
        "VpcId": "vpc-02c6f6694acc5b388",
        "Tags": [],
        "Entries": [
            {
                "IcmpTypeCode": {},
                "RuleNumber": 32767,
                "Protocol": "-1",
                "PortRange": {},
                "Egress": true,
                "RuleAction": "deny",
                "CidrBlock": "0.0.0.0/0"
            },
            {
                "IcmpTypeCode": {},
                "RuleNumber": 32767,
                "Protocol": "-1",
                "PortRange": {},
                "Egress": false,
                "RuleAction": "deny",
                "CidrBlock": "0.0.0.0/0"
            }
        ],
        "IsDefault": false
    }
}

Inbound Traffic:
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --ingress --rule-number 110 --protocol tcp --port-range From=22,To=22 --cidr-block 0.0.0.0/0 --rule-action allow
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --ingress --rule-number 100 --protocol tcp --port-range From=80,To=80 --cidr-block 0.0.0.0/0 --rule-action allow
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --ingress --rule-number 120 --protocol udp --port-range From=53,To=53 --cidr-block 0.0.0.0/0 --rule-action allow

Outbound Traffic:
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --egress --rule-number 110 --protocol tcp --port-range From=22,To=22 --cidr-bl ock 0.0.0.0/0 --rule-action allow
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --egress --rule-number 100 --protocol tcp --port-range From=80,To=80 --cidr-bl ock 0.0.0.0/0 --rule-action allow
# aws ec2 create-network-acl-entry --network-acl-id "acl-0254e40151a20d0ed" --egress --rule-number 120 --protocol udp --port-range From=53,To=53 --cidr-bl ock 0.0.0.0/0 --rule-action allow



STEP-6: Create EC2 security Groups i.e. SSH
# aws ec2 create-security-group --group-name "sg_ec2_ELB" --description "ELB SG PORT-80" --vpc-id "vpc-02c6f6694acc5b388"
{
    "GroupId": "sg-0bdcfebd258f16af5"
}
 
Enable a specific port on Security Group:

# aws ec2 authorize-security-group-ingress --group-id "sg-0bdcfebd258f16af5" --protocol tcp --port 80 --cidr 0.0.0.0/0

STEP-7: Create Database security Group i.e. 3306

STEP-8: Create Classic ELB and security Group i.e. HTTP
# aws elb create-load-balancer --load-balancer-name "ELBASOU" --listeners "Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80" --subnets "subnet-091331be378b2807a" "subnet-02201f4ce3f502164" --security-groups "sg-0bdcfebd258f16af5"

{
    "DNSName": "ELBASOU-1224408055.us-east-1.elb.amazonaws.com"
}

# aws elb configure-health-check --load-balancer-name "ELBASOU" --health-check Target=HTTP:80/index.html,Interval=10,UnhealthyThreshold=2,HealthyThreshold=2,Timeout=3

{
    "HealthCheck": {
        "HealthyThreshold": 2,
        "Interval": 10,
        "Target": "HTTP:80/index.html",
        "Timeout": 3,
        "UnhealthyThreshold": 2
    }
}


STEP-9: Create Launch Configuration and security group i.e. SSH-HTTP-HTTPS-DB-ICMP

# cat myuserdata.txt
#!/bin/bash
yum update -y
yum install http* --skip-broken -y
service httpd start
chkconfig httpd on
echo "Welcome to VPC ELB & AutoScaling" > /var/www/html/index.html

STEP-9.A: Create LC Group security Groups i.e. SSH-HTTP-DB-ICMP
# aws ec2 create-security-group --group-name "sg_lcgroup" --description "LCG_SSH_HTTP_DB" --vpc-id "vpc-02c6f6694acc5b388"
{
    "GroupId": "sg-08d3c85292e7c4073"
}

Enable a specific port on Security Group:

# aws ec2 authorize-security-group-ingress --group-id "sg-08d3c85292e7c4073" --protocol tcp --port 22 --cidr 0.0.0.0/0
# aws ec2 authorize-security-group-ingress --group-id "sg-08d3c85292e7c4073" --protocol tcp --port 80 --cidr 0.0.0.0/0
# aws ec2 authorize-security-group-ingress --group-id "sg-08d3c85292e7c4073" --protocol tcp --port 3306 --cidr 0.0.0.0/0

# aws autoscaling create-launch-configuration --launch-configuration-name "OU_LC" --key-name "nn_kk" --image-id "ami-0ff8a91507f77f867" --instance-type t2.micro --security-groups "sg-08d3c85292e7c4073" --user-data file://myuserdata.txt

STEP-10: Create AutoScaling & Configure Increate Scaling Group & Descrease Scaling Group & Map with Launch Configuration & Classic ELB

STEP-11: Launch Bastion Server part of Public Subnet which is part of one of the Availability Zone i.e. us-east-1a

# aws ec2 run-instances --image-id "ami-0ff8a91507f77f867" --count 1 --instance-type t2.micro --key-name nn_kk --security-group-ids "sg-08d3c85292e7c4073" --subnet-id "subnet-091331be378b2807a" --tag-specifications 'ResourceType=instance,Tags=[{Key=ToDoAdministration,Value=BastionServer}]'

STEP-12: Launch NAT Gateway part of another Public Subnet which is part of another Availability Zone i.e. us-east-1b

# aws ec2 create-nat-gateway --subnet-id "subnet-02201f4ce3f502164" --allocation-id eipalloc-03f5b5e944cb0edc7
{
    "NatGateway": {
        "NatGatewayAddresses": [
            {
                "AllocationId": "eipalloc-03f5b5e944cb0edc7"
            }
        ],
        "VpcId": "vpc-02c6f6694acc5b388",
        "State": "pending",
        "NatGatewayId": "nat-0d8c5ac32ad263553",
        "SubnetId": "subnet-02201f4ce3f502164",
        "CreateTime": "2018-09-06T14:22:11.000Z"
    }
}

# aws ec2 describe-subnets --region us-east-1 --subnet-ids "subnet-02201f4ce3f502164" --query "Subnets[*].{SubnetId:SubnetId, AvailabilityZone:AvailabilityZone}"
[
    {
        "SubnetId": "subnet-02201f4ce3f502164",
        "AvailabilityZone": "us-east-1b"
    }
]

Create a route in the route table that points all traffic (0.0.0.0/0) to the Internet gateway.
# aws ec2 create-route --route-table-id "rtb-0ddc10ca4812d56c9" --destination-cidr-block 0.0.0.0/0 --nat-gateway-id "nat-0d8c5ac32ad263553"
{
    "Return": true

}


STEP-9.A: Create LC Group security Groups i.e. DB
# aws ec2 create-security-group --group-name "sg_DB" --description "RDS" --vpc-id "vpc-02c6f6694acc5b388"

{
    "GroupId": "sg-000d422cd10e81ed8"
}

Enable a specific port on Security Group:

# aws ec2 authorize-security-group-ingress --group-id "sg-000d422cd10e81ed8" --protocol tcp --port 3306 --cidr 0.0.0.0/0

# aws rds create-db-instance --db-instance-identifier sg-cli-test --allocated-storage 20 --db-instance-class db.t2.small --engine mysql --master-username cloudcicd --master-user-password cloudcicd --db-name cloudcicd --port 3306 --db-subnet-group-name "subnet-084e774a2a5354d3c" --db-security-groups "sg-000d422cd10e81ed8"

# aws rds create-db-instance --db-instance-identifier sg-cli-test --allocated-storage 20 --db-instance-class db.t2.small --engine mysql --master-username cloudcicd --master-user-password cloudcicd --db-name cloudcicd --port 3306 --availability-zone us-east-1a --vpc-security-group-ids "sg-000d422cd10e81ed8"

#--------------------------------------------------------------------------------#
