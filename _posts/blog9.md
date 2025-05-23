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

Lastly, we see the "tag" section. This is the actual tag that can be used to search or reference this VPC resource within AWS cli or the AWS console. **_it DOES NOT have to be equal to the tag given by Terraform in the beginning_**. ~~This may have been a headache for me when I was playing around with it~~

Regarding the resource and item ID mentioned earlier, these can be referred to in later resources using curly braces.

![image](https://user-images.githubusercontent.com/20525440/68502164-a2135980-0214-11ea-9b49-e7c7d3411251.png)

Here we see the block used to create a subnet within the vpc shown earlier.

How do I know it's in that vpc?

>vpc_id = "${aws_vpc.project1-vpc.id}"

Is used to pull the vpc id from the resource we declared earlier. This allows us to make changes to variables accross multiple TF files without having to manually type in the ID for each resource block.

We can also use this syntax to specify variables from other .tf files.

I won't go into my full terraform code, but I will give a little bit of detail on what to do after.


### Terraform commands

Terraform has 4 basic commands that a user should run once their .tf files are configured.
>terraform validate

>terrform init

>terraform plan

>terraform apply

Terraform validate is a built-in syntax check for .tf files. Running it will print out in console regarding typos in resource names and the like.


![image](https://user-images.githubusercontent.com/20525440/68503431-ae4ce600-0217-11ea-8252-6f216259a297.png)

Terraform init exists to initliaze any necessary binaries in order to get your .tf to run
In this example shown above, we see that after specifying our provider, aws, terraform init is used to download the binaries necessary to create AWS resources declared in our .tf files.

For me specifically, make sure that they provider version that you have matches completely. 
```provider.aws: version = "~> 2.10"```
Here we see that the version must be specified with this syntax. If you only have 2.1 ~~It becomes a major headache~~, terraform init will tell you it cannot initialize your workspace due to being unable to download binaries for your provider.

![image](https://user-images.githubusercontent.com/20525440/68503883-bbb6a000-0218-11ea-877f-899120716f6f.png)

Terraform plan exists to show the changes that will be put in place. The above image is an example of the console print out for an aws instance with various other devices as a portion of it. 

It currently says that the printout is from a "terraform apply". Terraform plan used to be mandatory before executing the configuration, but now you can get a printout as soon as you run terraform apply.

I would still probably always use plan just to make sure things go smoothly
![image](https://user-images.githubusercontent.com/20525440/68503927-d9840500-0218-11ea-8188-e3534f26e248.png)

This shows how .tf configuration blocks are shown within terraform plan.


Terraform apply executes on configurations within the .tf file.
From there, it saves the IDs being referenced to a **_VERY IMPORTANT, VERY NECESSARY TO KEEP TRACK OF FILE_**


![image](https://user-images.githubusercontent.com/20525440/68505452-17365d00-021c-11ea-9aed-e2ca1f182330.png)
The .tf state can be viewed via terraform show. As stated before, it shows what specific strings are being referenced by terraform for specific variables.

If you're having trouble with things showning up ~~like accidentally making a typo in the availability zone and never catching it during terraform plan~~~ then it would be best to check the tfstate to make sure variables are being properly referenced.


Lastly, when working with a team, it may be best to use a remote state for your team's tfstate.
Through this, teams can work on components of infrastructure and ensure that they are running from the same terrraform outputs (the values of the variables shown, e.g. id = ___)___
Instructions and more on remote state [here](https://www.terraform.io/docs/state/remote.html)
