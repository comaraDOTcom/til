Terraform is a infrastructure as code tool.
The registry in terraform lists all the modules from different cloud providers we've created. As an example the modules for our stack are aws and snowflake. Terraform uses a synatax called HCL (Hasicorp configuration language?).

**ARN**= Amazon Resource Name

We can create modules in terraform, which will list them as part of the registry. Calling a module will create resources and may supply outputs.

### Example toy module
Let's add a module of an Amazon ECR instance. AWS will scale the docker container automatically ( we write and package our code as a docker container).

Keep it simple and use what terraform provide where possible: [Terraform-aws-ecr module](https://github.com/terraform-aws-modules/terraform-aws-ecr). 
Add a module to the of an ECR instance to the infrastructure repo.

```
module "conors-toy-example" {
	source = "terraform-aws-modules/ecr/aws"
	name="conor-ecr",
	version = "3.0.0" // version of our ecr module
	organization_read = true // 
	lamdba_account_ids = [ "XXXX"] // my aws account id
}
```



## Execute in terraform
```bash
terraform init
terraform plan
terraform apply
```

### if you want to teardown
```
terraform destroy
```
