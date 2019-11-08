# Blog 9 [11.9.2019]

The second week of our foray into infrastructure as code introduces us to another devops buzzword: terraform.

### Terraform by hashicorp
![image](https://banner2.cleanpng.com/20180529/szy/kisspng-terraform-hashicorp-microsoft-azure-infrastructure-5b0e0b6cc80963.2449977615276470848194.jpg)


Terraform is an application that seems to be the poster child of the "infrastructure as code" phenomenon.

Through various configuration files that are structured it's own native syntax, we can produce code to spin up machines that are exactly identical to each other, as well as change the amount of resources they individually use.

Terraform syntax is very similar to JSON, so much so they they even natively support JSON, as shown [here:](https://www.terraform.io/docs/configuration/syntax-json.html)

The only change being that the file must be named ```.tf.json``` as opposed to simply ```.tf```

On the topic of .tf files...


# Terraform configuration files.

Terraform mainly works by reading infrastructure configuration from .tf files.

Here is an example from my github 
