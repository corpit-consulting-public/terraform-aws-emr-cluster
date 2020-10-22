# terraform-aws-emr-cluster

A Terraform module to create an Amazon Web Services (AWS) Elastic MapReduce (EMR) cluster. Available through the [Terraform registry](https://registry.terraform.io/modules/dwp/emr-cluster/aws/0.2.2)   

## Usage

```hcl
data "template_file" "emr_configurations" {
  template = "${file("configurations/default.json")}"
}

module "emr" {
  source = "github.com/azavea/terraform-aws-emr-cluster?ref=0.1.0"

  name          = "DatarpocCluster"
  vpc_id        = "vpc-20f74844"
  release_label = "emr-5.9.0"

  applications = [
    "Hadoop",
    "Ganglia",
    "Spark",
    "Zeppelin",
  ]

  configurations = "${data.template_file.emr_configurations.rendered}"
  key_name       = "hector"
  subnet_id      = "subnet-e3sdf343"

  master_instance_group = {
      name           = "MasterInstanceGroup"
      instance_role  = "MASTER"
      instance_type  = "m3.xlarge"
      instance_count = "1"
    }
  core_instance_group = {
      name           = "CoreInstanceGroup"
      instance_role  = "CORE"
      instance_type  = "m3.xlarge"
      instance_count = "1"
      bid_price      = "0.30"
    }

  bootstrap_name = "runif"
  bootstrap_uri  = "s3://elasticmapreduce/bootstrap-actions/run-if"
  bootstrap_args = []
  log_uri        = "s3n://.../"

  project     = "Something"
  environment = "Staging"
}
```
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| additional\_master\_security\_group\_id | Additional security group for master node | string | n/a | yes |
| additional\_slave\_security\_group\_id | Additional security group for slave nodes | string | n/a | yes |
| bootstrap\_actions\_list |  | list | n/a | yes |
| configurations |  | string | n/a | yes |
| key\_name |  | string | n/a | yes |
| log\_uri |  | string | n/a | yes |
| name |  | string | n/a | yes |
| service\_access\_security\_group\_id | Security group with access to AWS Services over internet gateway. Applied to cluster service | string | n/a | yes |
| subnet\_id |  | string | n/a | yes |
| vpc\_id |  | string | n/a | yes |
| applications |  | list | `<list>` | no |
| core\_instance\_group |  | list | `<list>` | no |
| custom\_policy\_arns | List of policy arns to add to instance profile role | list | `<list>` | no |
| custom\_policy\_count | Number of custom policy arns in custom\_policy\_arns | string | `"0"` | no |
| environment |  | string | `"Unknown"` | no |
| keep\_job\_flow\_alive\_when\_no\_steps |  | string | `"true"` | no |
| master\_instance\_group |  | list | `<list>` | no |
| project |  | string | `"Unknown"` | no |
| release\_label |  | string | `"emr-5.8.0"` | no |
| step |  | map(string) | `<map>` | no |
| step\_args |  | list | `<list>` | no |

## Instance Group Example

```hcl
master:
    {
      name           = "MasterInstanceGroup"
      instance_role  = "MASTER"
      instance_type  = "m3.xlarge"
      instance_count = 1
    }
core:
    {
      name           = "CoreInstanceGroup"
      instance_role  = "CORE"
      instance_type  = "m3.xlarge"
      instance_count = "1"
      bid_price      = "0.30"
    },
```

## Outputs

| Name | Description |
|------|-------------|
| id |  |
| master\_public\_dns |  |
| master\_security\_group\_id |  |
| name |  |
| slave\_security\_group\_id |  |

