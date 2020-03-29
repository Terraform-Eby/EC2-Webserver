# EC2-Webserver

This is a Terraform code written to spin-up an ec2 instance with Apache installed. Below Terraform code will use existing security groups and keypair that is already exists in the account.


```
################################################################
# Creating EC2 Instance
################################################################

resource "aws_instance" "Webserver" {

    ami = "ami-0e01ce4ee18447327"
    instance_type = "t2.micro"
    key_name = "xxxx-provide-your-key-name-xxxx"
    availability_zone  = "us-east-2a"
    associate_public_ip_address = true
    user_data = file("instll-httpd.sh")
    vpc_security_group_ids = ["http-ssh-sg"]
    tags = {
        Name = "Webserver"
  }
}

output "ec2server-instance" {
  value = aws_instance.Webserver
}

```

* Install Apache and update index.html file using UserData

* Create a file install-http.sh and copy below contents. This file name is already specified in Terraform code above (user_data = file("instll-httpd.sh")).

```
#!/bin/bash

yum install httpd -y
service httpd restart
chkconfig httpd on
echo "<h1><center>Deployed From Terraform</center></h1>" > /var/www/html/index.html
```

* Initialize Terraform
```
$terraform init
```
* Check for changes that will be applied up on running the code.
```
$terraform plan
```
* Execute the code as follows
```
$terraform apply
```
* In case you want to remove all the applied changes.
```
$terraform destroy
```
