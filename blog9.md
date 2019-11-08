# Blog 9 [11.9.2019]

The second week of our foray into infrastructure as code introduces us to another devops buzzword: terraform.

## Terraform by hashicorp
![kisspng-terraform-hashicorp-microsoft-azure-infrastructure-5b0e0b6d0be980 0079130015276470850488](https://user-images.githubusercontent.com/20525440/68502909-71ccba80-0216-11ea-991a-e697e956e51a.png)



Terraform is an application that seems to be the poster child of the "infrastructure as code" phenomenon.

Through various configuration files that are structured it's own native syntax, we can produce code to spin up machines that are exactly identical to each other, as well as change the amount of resources they individually use.

Terraform syntax is very similar to JSON, so much so they they even natively support JSON, as shown [here.](https://www.terraform.io/docs/configuration/syntax-json.html)

The only change being that the file must be named ```.tf.json``` as opposed to simply ```.tf```

That being said:on the topic of .tf files...


### Terraform configuration files.

Terraform mainly works by reading infrastructure configuration from .tf files.

Here is an example from my github 
![image](https://user-images.githubusercontent.com/20525440/68497651-e77e5980-0209-11ea-956c-6202c6ecccab.png)

This is an example of a small block of terraform code.

>resource 

"Resource" a built-in terraform element that tells terraform to use the following block to create a resource using the following configuration variables, defines by the curly braces. The resource type "VPC" in this case and the resource name "project1-vpc" are used as an identifier for the Terraform module, but does not necessarily have significance ouside Terraform.

The inner configuration elements are in the block defined by the curly braces and are arguments needed to create the resources itself. 

So in the image shown, we declare to use the VPC resource from AWS, and from here on it, anything talking about "project1-vpc" is going to refer to the resource instance as shown in the image above. We then declare that this VPC will be able to use DNS, and consists of the 172.16../16 network.
Lastly, we see the "tag" section. This is the actual tag that can be used to search or reference this VPC resource within AWS cli or the AWS console. it DOES NOT have to be equal to the tag given by Terraform in the beginning.

Regarding the resource and item ID mentioned earlier, these can be referred to in later resources using curly braces.

![image](https://user-images.githubusercontent.com/20525440/68502164-a2135980-0214-11ea-9b49-e7c7d3411251.png)

Here we see the block used to create a subnet within the vpc shown earlier.

How do I know it's in that vpc?

>vpc_id = "${aws_vpc.project1-vpc.id}"

Is used to pull the vpc id from the resource we declared earlier. This allows us to make changes to variables accross multiple TF files without having to manually type in the ID for each resource block.

I won't go into my full terraform code, but I will give a little bit of detail on what to do after.


### Terraform commands

Terraform has 4 basic commands that a user should run once their .tf files are configured.
>terraform validate
terrform init
terraform plan
terraform apply
