# 5 Things You Can Do to Enhance Your EC2 Instances on AWS with Terraform

Security is even more important in the cloud: it’s not just IT provisioning resources, but also developers, plus whoever else has access to launch resources—and nowadays, far more people have access. Also the rate of storing sensitive and confidential data is ever increasing. It’s vital for organizations to take steps to ensure full control of AWS security.

Some of these measures are as simple as enabling security services, but some measures require better training or getting help. The end goal should be to make security part of the operation—not an afterthought.

Let’s talk about  measures you can take to secure your environment.

## 1. Restricting incoming traffic for unauthorized ports in security groups
Security groups should only allow unrestricted incoming traffic for authorized ports.
Allowing unrestricted incoming traffic for unauthorized ports poses a significant security risk as it increases the potential attack surface for your systems.

## Steps to Follow:
- Check current security group configurations.
- Identify ports that are currently allowed unrestricted incoming traffic.
- Compare these ports to the list of authorized ports. Only authorized ports (80 and 443) should be allowed unrestricted incoming traffic.

## Solution:

- Review the list of authorized ports and update the security group configurations to only allow incoming traffic for these ports.
- Implement proper security measures to prevent unauthorized access to these ports.
- Test the new configurations to ensure that they are working as expected

## Implement solution with Terraform

``` terraform
resource "aws_security_group" "docker" {
  name        = "ssh-http-https"
  description = "Allow SSH-HTTP-HTTPS for inbound traffic"
  vpc_id = module.vpc.vpc_id

  ingress {
    description = "HTTPS from VPC"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP from VPC"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "SSH from VPC"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "allow_ssh-http-https"
  }
}
```



Sources: 
https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-controls-reference.html#fsbp-account-1
