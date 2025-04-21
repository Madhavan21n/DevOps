# terraform {
#   required_providers {
#     aws = {
#       source = "hashicorp/aws"
#       version = "5.94.1"
#     }
#   }
# }

# provider "aws" {
#   # Configuration options
#   region = "us-east-1"
# }

#resource "aws_vpc" "vpc" {
  #cidr_block       = "10.0.0.0/16"
  #instance_tenancy = "default"

  #tags = {
    #Name = "my_vpc"
  #}
#}

# resource "aws_subnet" "pub" {
#   vpc_id     = aws_vpc.vpc.id
#   cidr_block = "10.0.1.0/24"

#   tags = {
#     Name = "pub-sub"
#   }
# }

# resource "aws_subnet" "prv" {
#   vpc_id     = aws_vpc.vpc.id
#   cidr_block = "10.0.2.0/24"

#   tags = {
#     Name = "prv-sub"
#   }
# }

# resource "aws_route_table" "pub-tab" {
#   vpc_id = aws_vpc.vpc.id

#   #route = []

#   tags = {
#     Name = "pub-rot"
#   }
# }

# resource "aws_route_table" "prv-tab" {
#   vpc_id = aws_vpc.vpc.id

#   #route = []

#   tags = {
#     Name = "prv-rot"
#   }
# }

# resource "aws_internet_gateway" "igw" {
#   vpc_id = aws_vpc.vpc.id

#   tags = {
#     Name = "main"
#   }
# }

# resource "aws_eip" "eip" {
#   #instance = aws_instance.web.id
#   domain   = "vpc"
# }


# resource "aws_nat_gateway" "nat" {
#   allocation_id = aws_eip.eip.id
#   subnet_id     = aws_subnet.pub.id

#   tags = {
#     Name = "ngw"
#   }

#   # To ensure proper ordering, it is recommended to add an explicit dependency
#   # on the Internet Gateway for the VPC.
#   depends_on = [aws_internet_gateway.igw]
# }

# resource "aws_route_table_association" "pub" {
#   subnet_id      = aws_subnet.pub.id
#   route_table_id = aws_route_table.pub-tab.id
# }

# resource "aws_route_table_association" "prv" {
#   subnet_id      = aws_subnet.prv.id
#   route_table_id = aws_route_table.prv-tab.id
# }

# resource "aws_route" "pubr" {
#   route_table_id            = aws_route_table.pub-tab.id
#   destination_cidr_block    = "0.0.0.0/0"
#   gateway_id = aws_internet_gateway.igw.id
# }

# resource "aws_route" "prvr" {
#   route_table_id            = aws_route_table.prv-tab.id
#   destination_cidr_block    = "0.0.0.0/0"
#   nat_gateway_id = aws_nat_gateway.nat.id
# }

# resource "aws_instance" "default" {
#   ami           = "ami-0c02fb55956c7d316" # Amazon Linux 2 (us-east-1)
#   instance_type = "t2.micro"

#   tags = {
#     Name = "DefaultEC2Instance"
#   }
# }

# # 👇 Key Pair from your local public key file
# resource "aws_key_pair" "deployer" {
#   key_name   = "terraform-key"
#   public_key = file("~/.ssh/id_rsa.pub") # path to your public key
# }

# 👇 Security group allowing SSH (port 22)
# resource "aws_security_group" "allow_ssh" {
#   name        = "allow_ssh"
#   description = "Allow SSH inbound traffic"

#   ingress {
#     description = "SSH from anywhere"
#     from_port   = 22
#     to_port     = 22
#     protocol    = "tcp"
#     cidr_blocks = ["0.0.0.0/0"]
#   }

#   egress {
#     description = "Allow all outbound"
#     from_port   = 0
#     to_port     = 0
#     protocol    = "-1"
#     cidr_blocks = ["0.0.0.0/0"]
#   }
# }

# 👇 EC2 instance with the key pair and security group
# resource "aws_instance" "web" {
#   ami                    = "ami-0c02fb55956c7d316" # Amazon Linux 2 in us-east-1
#   instance_type          = "t2.micro"
#   # key_name               = aws_key_pair.deployer.key_name
#   vpc_security_group_ids = [aws_security_group.allow_ssh.id]

#   tags = {
#     Name = "TerraformEC2WithKey"
#   }
# }
