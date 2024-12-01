## Terraform

1. What does Terraform code contain?

<details>
  <summary>Answer</summary>

Cloud provider resources, as well as provisioning for the resources created.

</details>

2. How to store tfstate in Terraform?

<details>
  <summary>Answer</summary>

For example, you can store tfstate in the team's git repository. Another option is to store it in a specialized Terraform Backend.

</details>

3. Terraform Backend. What better?

<details>
  <summary>Answer</summary>

Depends on the storage requirements of the tfstate.

- AWS S3 - Standard (with locking via DynamoDB) - Stores state as a given key in a given bucket on Amazon S3. This backend also supports state locking and consistency checking via DynamoDB.

- terraform enterprise — Standard (without locking).

- etcd — Standard (without locking). Stores state in etcd 2.x at the given path.

- etcdv3 — Standard (with locking). Stores state in etcd storage as K/V with the given prefix.

- gcs — Standard (with locking). Stores state as an object in a custom prefix in a given bucket in Google Cloud Storage (GCS). This backend also supports state locking.

- Gitlab Terraform state (with locking). Stores state in a Gitlab Terraform state store using the HTTP protocol and Gitlab permissions for access.

There are also other Backends for Terraform.

</details>

4. How to add existing resources to tfstate?

<details>
  <summary>Answer</summary>

```
terraform import [options] ADDRESS ID
```
1. For example, we create a directory and initialize the future infrastructure:
```
mkdir terraform-test
cd terraform-test
terraform init
vi main.tf
```
2. Add to main.tf this code:
```
provider "aws" {
  region = "us-west-1"
  profile = "tyx-local"
}
resource "aws_s3_bucket" "sample_bucket" {
  bucket = "tyx-local-bucket"
  acl = "public"
}
```
3. Launch import resources:
```
terraform import aws_s3_bucket.sample_bucket tyx-local-bucket
```

</details>

5. Why is needed `terraform taint`?

<details>
  <summary>Answer</summary>

Command `terraform taint` will mark the infrastructure resource to be deleted and recreated the next time the command is run `terraform apply`. 

</details>

6. How to test terraform?

<details>
  <summary>Answer</summary>

`terraforn plan` will perform a check of the current code. 

</details>

7. What is a module in terraform? What is it for?

<details>
  <summary>Answer</summary>

A Module in Terraform is a Terraform configuration package that can be used to reconfigure infrastructure components, as well as the basic organization of Terraform code into directories. When a module is included, it is given a name.

</details>

8. How to store variables in Terraform?

<details>
  <summary>Answer</summary>

*main.tf* - The main configuration file that describes which instances need to be created.
*variables.tf* - configuration with variable descriptions and default values. If default values ​​are not specified, they are mandatory.
*terraform.tfvars* - configuration with variable values. Often a secret file, so push to public repositories with caution.
*outputs.tf* - Description of output variables. An optional file, but very convenient to select the necessary parameters from the created instance, for example, the IP of the instance created in the cloud.

</details>

9. How to convert Kubernetes yaml manifest to HCL using Linux and terraform?

<details>
  <summary>Answer</summary>

For example:
```
echo 'yamldecode(file("filename.yaml"))' | terraform console
```

</details>

10. What is Workspaces in Terraform?

<details>
  <summary>Answer</summary>

[Workspaces](https://developer.hashicorp.com/terraform/language/state/workspaces#using-workspaces) in Terraform is the ability to manage state files. A Workspace contains everything needed to manage a set of infrastructure, and individual workspaces function as completely separate working directories. With Workspaces, it is possible to manage multiple infrastructure environments.

</details>

11. What is terragrunt used for?

<details>
  <summary>Answer</summary>

Terragrunt — is a wrapper for Terraform that addresses issues related to scaling and reusing code to configure infrastructure. It allows reuse of configuration parameters and supports multi-level configurations and dependencies.

</details>

12. What is the difference `count` and `for_each`?

<details>
  <summary>Answer</summary>

`count` is an iteration over a list that contains integer elements, `for_each` is an iteration over the root keys of a dictionary that can contain data of any type.

```
resource "aws_instance" "web" {
  count = 3
 
  instance_type = "t2.micro"
  ami           = data.aws_ami.debian_buster.id
  tags = {
    Name = "WebServer-${count.index + 1}"
  }
}
```
The resource description above will create 3 identical EC2 instances, changing the name to indicate the current state of the counter. `count` starts counting from 0, so to have 1 EC2 instance with index 1 in its name, `1` is added.

```
resource "aws_instance" "server" {
  for_each = {
    web = { type = "t2.micro", public_ip = true },
    db  = { type = "m5.large", public_ip = false }
  }
 
  instance_type = each.value["type"]
  ami           = data.aws_ami.debian_buster.id
  associate_public_ip_address = each.value["public_ip"]
  tags = {
    Name = "each.key"
  }
}
```
The resource above will create 2 EC2 instances iterating over the keys `each.key` and using the values ​​of the nested dictionaries in the EC2 configuration.

</details>
