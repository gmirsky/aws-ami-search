# aws-ami-search
Search for a specific AWS AMI

Call the module as follows:

    module "aws-ami-search" {
    source = "../aws-ami-search"
    os     = "windows-2019-base"
    }

Obtain the AMI value value as shown in the example below calling **module.aws-ami-search.ami_id**

    resource "aws_instance" "server-1" {
    instance_type        = var.instance_type
    iam_instance_profile = aws_iam_instance_profile.my_profile.name
    ami                  = module.aws-ami-search.ami_id
    tags = merge(
        var.common_tags,
        var.project_tags,
        tomap({ "Name" = "${var.instance_prefix}1" })
    )
    root_block_device {
        volume_type           = "gp2"
        volume_size           = 40
        delete_on_termination = true
        encrypted             = true
    }
    ebs_block_device {
        device_name           = "/dev/xvdb"
        volume_type           = "gp2"
        volume_size           = 100
        delete_on_termination = true
        encrypted             = true
    }
    availability_zone           = var.aws_availability_zone
    subnet_id                   = var.subnet_id
    vpc_security_group_ids      = var.vpc_security_group_ids
    associate_public_ip_address = "false"
    key_name                    = var.key_name
    }