# Terraform-aws-db

# AWS Infrastructure Provisioning with Terraform

## Table of Contents
- [Introduction](#introduction)
- [Usage](#usage)
- [Module Inputs](#module-inputs)
- [Module Outputs](#module-outputs)
- [License](#license)

## Introduction
This module is basically combination of Terraform open source and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.
## Usage
To use this module, you can include it in your Terraform configuration. Here's an example of how to use it:

## Examples

## Example: MariaDB

```hcl
module "mariadb" {
  source                          = "git@github.com:slovink/terraform-aws-db.git"
  name                            = "mariadb"
  environment                     = "test"
  label_order                     = ["environment", "name"]
  engine                          = "MariaDB"
  engine_version                  = "10.6.10"
  instance_class                  = "db.m5.large"
  engine_name                     = "MariaDB"
  allocated_storage               = 50
  db_name                         = "test"
  username                        = "user"
  password                        = "esfsgcGdfawAhdxtfjm!"
  port                            = "3306"
  maintenance_window              = "Mon:00:00-Mon:03:00"
  backup_window                   = "03:00-06:00"
  multi_az                        = false
  vpc_id                          = module.vpc.id
  allowed_ip                      = [module.vpc.vpc_cidr_block]
  allowed_ports                   = [3306]
  family                          = "mariadb10.6"
  backup_retention_period         = 0
  enabled_cloudwatch_logs_exports = ["audit", "general"]
  subnet_ids                      = module.private_subnets.public_subnet_id
  publicly_accessible             = true
  major_engine_version            = "10.6"
  deletion_protection             = false
  ssm_parameter_endpoint_enabled  = true
}
```
## Example: mysql-complete
```hcl
module "mysql" {
  source                          = "git@github.com:slovink/terraform-aws-db.git?ref=v1.0.0"
  name                            = "mysql"
  environment                     = "test"
  label_order                     = ["environment", "name"]
  engine                          = "mysql"
  engine_version                  = "8.0.28"
  instance_class                  = "db.m6i.xlarge."
  allocated_storage               = 5
  vpc_id                          = module.vpc.id
  allowed_ip                      = [module.vpc.vpc_cidr_block]
  allowed_ports                   = [3306]
  db_name                         = "test"
  username                        = "user"
  password                        = "esfsgcGdfawAhdxtfjm!"
  port                            = "3306"
  maintenance_window              = "Mon:00:00-Mon:03:00"
  backup_window                   = "03:00-06:00"
  multi_az                        = false
  backup_retention_period         = 7
  enabled_cloudwatch_logs_exports = ["audit", "general"]
  subnet_ids                      = module.subnets.public_subnet_id
  publicly_accessible             = true
  family                          = "mysql8.0"
  major_engine_version            = "8.0"
  deletion_protection             = false

  parameters = [
    {
      name  = "character_set_client"
      value = "utf8"
    },
    {
      name  = "character_set_server"
      value = "utf8"
    }
  ]

  options = [
    {
      option_name = "MARIADB_AUDIT_PLUGIN"

      option_settings = [
        {
          name  = "SERVER_AUDIT_EVENTS"
          value = "CONNECT"
        },
        {
          name  = "SERVER_AUDIT_FILE_ROTATIONS"
          value = "37"
        },
      ]
    },
  ]
  ssm_parameter_endpoint_enabled = true
}
```
## Example: oracle_db
```hcl
module "oracle" {
  source                              = "git@github.com:slovink/terraform-aws-db.git?ref=v1.0.0"
  name                                = "oracle"
  environment                         = "test"
  label_order                         = ["environment", "name"]
  engine                              = "oracle-ee"
  engine_version                      = "19"
  instance_class                      = "db.t3.medium"
  engine_name                         = "oracle-ee"
  allocated_storage                   = 50
  storage_encrypted                   = true
  family                              = "oracle-ee-19"
  db_name                             = "test"
  username                            = "admin"
  password                            = "esfsgcGdfawAhdxtfjm!"
  port                                = "1521"
  maintenance_window                  = "Mon:00:00-Mon:03:00"
  backup_window                       = "03:00-06:00"
  multi_az                            = false
  vpc_id                              = module.vpc.id
  allowed_ip                          = [module.vpc.vpc_cidr_block]
  allowed_ports                       = [1521]
  backup_retention_period             = 0
  enabled_cloudwatch_logs_exports     = ["audit"]
  subnet_ids                          = module.private_subnets.public_subnet_id
  publicly_accessible                 = true
  major_engine_version                = "19"
  deletion_protection                 = false
  iam_database_authentication_enabled = false
  ssm_parameter_endpoint_enabled      = true
}
```
## Example: postgreSQL
```hcl
module "postgresql" {
  source                          = "git@github.com:slovink/terraform-aws-db.git?ref=v1.0.0"
  name                            = "postgresql"
  environment                     = "test"
  label_order                     = ["environment", "name"]
  engine                          = "postgres"
  engine_version                  = "14.6"
  instance_class                  = "db.t3.medium"
  allocated_storage               = 50
  engine_name                     = "postgres"
  storage_encrypted               = true
  family                          = "postgres14"
  db_name                         = "test"
  username                        = "dbname"
  password                        = "esfsgcGdfawAhdxtfjm!"
  port                            = "5432"
  maintenance_window              = "Mon:00:00-Mon:03:00"
  backup_window                   = "03:00-06:00"
  multi_az                        = false
  vpc_id                          = module.vpc.id
  allowed_ip                      = [module.vpc.vpc_cidr_block]
  allowed_ports                   = [5432]
  backup_retention_period         = 0
  enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]
  subnet_ids                      = module.private_subnets.public_subnet_id
  publicly_accessible             = true
  major_engine_version            = "14"
  deletion_protection             = false
  ssm_parameter_endpoint_enabled  = true
}
```
## Example: replica-mysql
```hcl
module "mysql" {
  source                          = "git@github.com:slovink/terraform-aws-db.git?ref=v1.0.0"
  name                            = "rds"
  environment                     = "test"
  label_order                     = ["environment", "name"]
  enabled                         = true
  engine                          = "mysql"
  engine_version                  = "8.0"
  instance_class                  = "db.t4g.large"
  replica_instance_class          = "db.t4g.large"
  allocated_storage               = 20
  identifier                      = ""
  snapshot_identifier             = ""
  kms_key_id                      = ""
  enabled_read_replica            = true
  enabled_replica                 = true
  db_name                         = "replica"
  username                        = "replica_mysql"
  password                        = "clkjvnsdikjhdsijfsdli"
  port                            = 3306
  maintenance_window              = "Mon:00:00-Mon:03:00"
  backup_window                   = "03:00-06:00"
  multi_az                        = true
  vpc_id                          = module.vpc.id
  allowed_ip                      = [module.vpc.vpc_cidr_block]
  allowed_ports                   = [3306]
  backup_retention_period         = 1
  enabled_cloudwatch_logs_exports = ["general"]
  subnet_ids                      = module.subnets.public_subnet_id
  publicly_accessible             = false
  family                          = "mysql8.0"
  major_engine_version            = "8.0"
  auto_minor_version_upgrade      = false
  deletion_protection             = false
  ssm_parameter_endpoint_enabled  = true
}
```
## Module Inputs

- `name`: A name for your db.
- `engine`: The database engine to use.
- `engine_version`: The engine version to use.
- `instance_class` :  The instance type of the RDS instance.
- `engine_name` : The name of the database to create when the DB instance is created.
- `allocated_storage` : The allocated storage in gibibytes.
- `db_name` : The name of the database to create when the DB instance is created.
- `username` :  Username for the master DB user.
- `passwoed` : Password for the master DB user.
- `port` :The port on which the DB accepts connections.
- `maintenance_window` :  The window to perform maintenance in.
- `backup_window` : The daily time range (in UTC) during which automated backups are created if they are enabled.
- `multi_az` :  Specifies if the RDS instance is multi-AZ
- `enabled_cloudwatch_logs_exports` :  Set of log types to enable for exporting to CloudWatch logs.
- `major_engine_version` : Specifies the major version of the engine that this option group should be associated with.
- `allocated_storage` :  The allocated storage in gibibytes.
- `multi_az` :  Specifies if the RDS instance is multi-AZ
- `backup_retention_period`: The days to retain backups for.
- `identifier` :  The name of the RDS instance, if omitted, Terraform will assign a random, unique identifier.
- `snapshot_identifier` : Specifies whether or not to create this database from a snapshot.

- For security group settings, you can configure the ingress and egress rules using variables like:

## Module Outputs
- `db_instance_arn` : The ARN of the RDS instance.
- `db_instance_availability_zone`: The availability zone of the RDS instance.
- `db_instance_endpoint` : The connection endpoint.
- `db_instance_engine`: The database engine.
- `db_instance_id` : The RDS instance ID.
- `db_instance_address` : db_instance_address.
- `db_instance_hosted_zone_id` : The canonical hosted zone ID of the DB instance.
- `db_instance_status` : The RDS instance status
- `db_instance_name` : The database name
- `master_db_instance_resource_id` : The RDS Resource ID of this instance.
- `master_db_instance_username` : The master username for the database.
- `master_db_instance_password` : The database password.
- `master_db_instance_port` : The database port.
- `master_db_subnet_group_id` : The db subnet group name.
- `master_db_instance_cloudwatch_log_groups` : Map of CloudWatch log groups created and their attributes.

- Other relevant security group outputs (modify as needed).

## Example
For detailed examples on how to use this module, please refer to the '[example](https://github.com/slovink/terraform-aws-db/tree/master/example)' directory within this repository.

## Author
Your Name Replace '[License Name]' and '[Your Name]' with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/slovink/terraform-aws-db/blob/master/LICENSE) file for details.