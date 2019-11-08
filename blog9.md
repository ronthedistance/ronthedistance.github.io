# Blog 9 [11.9.2019]

The second week of our foray into infrastructure as code introduces us to another devops buzzword: terraform.

### Terraform by hashicorp
![image](https://banner2.cleanpng.com/20180529/szy/kisspng-terraform-hashicorp-microsoft-azure-infrastructure-5b0e0b6cc80963.2449977615276470848194.jpg)


Terraform is an application that seems to be the poster child of the "infrastructure as code" phenomenon.

Through various configuration files that are structured it's own native syntax, we can produce code to spin up machines that are exactly identical to each other, as well as change the amount of resources they individually use.

Terraform syntax is very similar to JSON, so much so they they even natively support JSON, as shown [here.](https://www.terraform.io/docs/configuration/syntax-json.html)

The only change being that the file must be named ```.tf.json``` as opposed to simply ```.tf```

That being said:on the topic of .tf files...


# Terraform configuration files.

Terraform mainly works by reading infrastructure configuration from .tf files.

Here is an example from my github 
![image](https://user-images.githubusercontent.com/20525440/68497651-e77e5980-0209-11ea-956c-6202c6ecccab.png)

This is an example of a small block of terraform code.

>resource 
"Resource" a built-in terraform element that tells terraform to use the following block to create a resource using the following configuration variables, defines by the curly braces. The resource type "VPC" in this case and the resource name "project1-vpc" are used as an identifier for the Terraform module, but does not necessarily have significance ouside Terraform.

The inner configuration elements are in the block defined by the curly braces and are arguments needed to create the resources itself. 

So in the image shown, we declare to use the VPC resource from AWS, and from here on it, anything talking about "project1-vpc" is going to refer to the resource instance as shown in the image above. We then declare that this VPC will be able to use DNS, and consists of the 172.16../16 network.
Lastly, we see the "tag" section. This is the actual tag that can be used to search or reference this VPC resource within AWS cli or the AWS console. it DOES NOT have to be equal to the tag given by Terraform in the beginning.


provider "aws" {
  profile    = "default"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-2757f631"
  instance_type = "t2.micro"
}

# create a VPC
resource "aws_vpc" "project1-vpc" {
  cidr_block           = "172.16.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "project1_vpc"
  }
}