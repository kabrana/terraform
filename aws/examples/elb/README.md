# Work with AWS ELB via terraform

A terraform module for making ELB.


## Usage
----------------------
Import the module and retrieve with ```terraform get``` or ```terraform get --update```. Adding a module resource to your template, e.g. `main.tf`:

```
#
# MAINTAINER Vitaliy Natarov "vitaliy.natarov@yahoo.com"
#
terraform {
  required_version = "~> 0.13.5"
}

provider "aws" {
  region = "us-east-1"
}

module "elb" {
  source      = "../../modules/elb"
  name        = "TEST"
  environment = "stage"

  enable_elb         = true
  elb_name           = "my-elb-here"
  security_groups    = []
  subnets            = []
  availability_zones = ["us-east-1a", "us-east-1b", "us-east-1c"]

  access_logs = []
  listener = [
    {
      instance_port     = "80"
      instance_protocol = "HTTP"
      lb_port           = "80"
      lb_protocol       = "HTTP"
    },
    #    {
    #        instance_port      = 443
    #        instance_protocol  = "https"
    #        lb_port            = 443
    #        lb_protocol        = "https"
    #        ssl_certificate_id = "${var.elb_certificate}"
    #    },
  ]
  health_check = [
    {
      target              = "HTTP:80/"
      interval            = 30
      healthy_threshold   = 2
      unhealthy_threshold = 2
      timeout             = 5
    }
  ]

  # Enable
  enable_lb_cookie_stickiness_policy_http = true

  # Enable
  enable_app_cookie_stickiness_policy_http = true

  tags = map("Env", "stage", "Orchestration", "Terraform")
}
```

## Module Input Variables
----------------------
- `name` - Name to be used on all resources as prefix (`default = TEST`)
- `environment` - Environment for service (`default = STAGE`)
- `tags` - A list of tag blocks. (`default = ""`)
- `enable_elb` - Enable ELB usage (`default = ""`)
- `elb_name` - The name of the ELB. By default generated by Terraform. (`default = ""`)
- `availability_zones` - Availability zones for AWS ASG (`default = null`)
- `security_groups` - A list of security group IDs to assign to the ELB. Only valid if creating an ELB within a VPC (`default = ""`)
- `subnets` - A list of subnet IDs to attach to the ELB (`default = null`)
- `elb_internal` - If true, ELB will be an internal ELB (`default = ""`)
- `cross_zone_load_balancing` - Enable cross-zone load balancing. Default: true (`default = True`)
- `idle_timeout` - The time in seconds that the connection is allowed to be idle. Default: 60 (`default = 60`)
- `connection_draining` - Boolean to enable connection draining. Default: false (`default = ""`)
- `connection_draining_timeout` - The time in seconds to allow for connections to drain. Default: 300 (`default = 300`)
- `access_logs` - An access logs block. Uploads access logs to S3 bucket (`default = ""`)
- `listener` - A list of Listener block (`default = ""`)
- `health_check` -  Health check (`default = ""`)
- `enable_elb_attachment` - Enable elb_attachment usage (`default = ""`)
- `instances` -  Instances ID to add them to ELB (`default = ""`)
- `elb_id` - ID of ELB (`default = ""`)
- `enable_lb_cookie_stickiness_policy_http` - Enable lb cookie stickiness policy http. If set true, will add it, else will use https (`default = True`)
- `lb_cookie_stickiness_policy_http_name` - Name for lb_cookie_stickiness_policy_http (`default = ""`)
- `lb_cookie_stickiness_policy_https_name` - Name for lb_cookie_stickiness_policy_https (`default = ""`)
- `enable_app_cookie_stickiness_policy_http` - Enable app cookie stickiness policy http. If set true, will add it, else will use https (`default = True`)
- `app_cookie_stickiness_policy_http_name` - Name for app_cookie_stickiness_policy_http (`default = ""`)
- `app_cookie_stickiness_policy_https_name` - Name for app_cookie_stickiness_policy_http (`default = ""`)
- `http_lb_port` - Set http lb port for lb_cookie_stickiness_policy_http|app_cookie_stickiness_policy_http policies (`default = 80`)
- `https_lb_port` - Set https lb port for lb_cookie_stickiness_policy_http|app_cookie_stickiness_policy_http policies (`default = 443`)
- `cookie_expiration_period` - Set cookie expiration period (`default = 600`)
- `cookie_name` - Set cookie name (`default = SessionID`)

## Module Output Variables
----------------------


## Authors

Created and maintained by [Vitaliy Natarov](https://github.com/SebastianUA). An email: [vitaliy.natarov@yahoo.com](vitaliy.natarov@yahoo.com).

## License

Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/terraform/blob/master/LICENSE) for full details.
