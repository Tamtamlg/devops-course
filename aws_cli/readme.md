# Task 1

```bash
aws ec2 create-vpc --cidr-block 10.10.0.0/16

{
    "Vpc": {
        "CidrBlock": "10.10.0.0/16",
        "DhcpOptionsId": "dopt-03a1249941d02b202",
        "State": "pending",
        "VpcId": "vpc-012c361d82376f53c",
        "OwnerId": "738339644517",
        "InstanceTenancy": "default",
        "Ipv6CidrBlockAssociationSet": [],
        "CidrBlockAssociationSet": [
            {
                "AssociationId": "vpc-cidr-assoc-04641d9cdb009b08c",
                "CidrBlock": "10.10.0.0/16",
                "CidrBlockState": {
                    "State": "associated"
                }
            }
        ],
        "IsDefault": false
    }
}
```

```bash
aws ec2 create-tags --resources vpc-012c361d82376f53c --tags Key=Name,Value=myvpc01
```

```bash
aws ec2 create-subnet --vpc-id vpc-012c361d82376f53c --cidr-block 10.10.1.0/24

{
    "Subnet": {
        "AvailabilityZone": "eu-west-2c",
        "AvailabilityZoneId": "euw2-az1",
        "AvailableIpAddressCount": 251,
        "CidrBlock": "10.10.1.0/24",
        "DefaultForAz": false,
        "MapPublicIpOnLaunch": false,
        "State": "available",
        "SubnetId": "subnet-09350d5e6e62dff32",
        "VpcId": "vpc-012c361d82376f53c",
        "OwnerId": "738339644517",
        "AssignIpv6AddressOnCreation": false,
        "Ipv6CidrBlockAssociationSet": [],
        "SubnetArn": "arn:aws:ec2:eu-west-2:738339644517:subnet/subnet-09350d5e6e62dff32",
        "EnableDns64": false,
        "Ipv6Native": false,
        "PrivateDnsNameOptionsOnLaunch": {
            "HostnameType": "ip-name",
            "EnableResourceNameDnsARecord": false,
            "EnableResourceNameDnsAAAARecord": false
        }
    }
}
```

```bash
aws ec2 create-tags --resources subnet-09350d5e6e62dff32 --tags Key=Name,Value=public
```

```bash
aws ec2 create-subnet --vpc-id vpc-012c361d82376f53c --cidr-block 10.10.2.0/24

{
    "Subnet": {
        "AvailabilityZone": "eu-west-2c",
        "AvailabilityZoneId": "euw2-az1",
        "AvailableIpAddressCount": 251,
        "CidrBlock": "10.10.2.0/24",
        "DefaultForAz": false,
        "MapPublicIpOnLaunch": false,
        "State": "available",
        "SubnetId": "subnet-0d2b6347dcdf29456",
        "VpcId": "vpc-012c361d82376f53c",
        "OwnerId": "738339644517",
        "AssignIpv6AddressOnCreation": false,
        "Ipv6CidrBlockAssociationSet": [],
        "SubnetArn": "arn:aws:ec2:eu-west-2:738339644517:subnet/subnet-0d2b6347dcdf29456",
        "EnableDns64": false,
        "Ipv6Native": false,
        "PrivateDnsNameOptionsOnLaunch": {
            "HostnameType": "ip-name",
            "EnableResourceNameDnsARecord": false,
            "EnableResourceNameDnsAAAARecord": false
        }
    }
}
```

```bash
aws ec2 create-tags --resources subnet-0d2b6347dcdf29456 --tags Key=Name,Value=private
```

```bash
aws ec2 create-internet-gateway

{
    "InternetGateway": {
        "Attachments": [],
        "InternetGatewayId": "igw-06d2474d9ac882dc1",
        "OwnerId": "738339644517",
        "Tags": []
    }
}
```

```bash
aws ec2 create-tags --resources igw-06d2474d9ac882dc1 --tags Key=Name,Value=public 
```

```bash
aws ec2 attach-internet-gateway --internet-gateway-id igw-06d2474d9ac882dc1 --vpc-id vpc-012c361d82376f53c
```

```bash
aws ec2 create-route-table --vpc-id vpc-012c361d82376f53c

{
    "RouteTable": {
        "Associations": [],
        "PropagatingVgws": [],
        "RouteTableId": "rtb-0c5ed9a30217e2ad4",
        "Routes": [
            {
                "DestinationCidrBlock": "10.10.0.0/16",
                "GatewayId": "local",
                "Origin": "CreateRouteTable",
                "State": "active"
            }
        ],
        "Tags": [],
        "VpcId": "vpc-012c361d82376f53c",
        "OwnerId": "738339644517"
    }
}
```

```bash
aws ec2 create-tags --resources rtb-0c5ed9a30217e2ad4 --tags Key=Name,Value=public
```

```bash
aws ec2 create-route --route-table-id rtb-0c5ed9a30217e2ad4 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-06d2474d9ac882dc1

{
    "Return": true
}
```

```bash
aws ec2 associate-route-table --route-table-id rtb-0c5ed9a30217e2ad4 --subnet-id subnet-09350d5e6e62dff32

{
    "AssociationId": "rtbassoc-0093a40e5b1d78db9",
    "AssociationState": {
        "State": "associated"
    }
}
```

# Task 2
```bash
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-012c361d82376f53c" "Name=tag:Name,Values=private" --query "Subnets[*].CidrBlock"

[
    "10.10.2.0/24"
]
```

# Task 3
```bash
aws ec2 describe-images --owners amazon --filters 'Name=description,Values=Amazon Linux 2 *' 'Name=architecture,Values=x86_64' --query 'sort_by(Images, &CreationDate)[-1].{ID:ImageId,Architecture:Architecture,Owner:ImageOwnerAlias,OS:Description}' --region us-west-2
```
