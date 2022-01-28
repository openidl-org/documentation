References – Inputs
===================

Input Template
--------------

| #The following inputs should be via git secrets as they contain
  sensitive data. refer to README.md
| ##################start of sensitive data that goes to git
  secrets###################
| aws_account_number = "" #mandatory
| aws_access_key = "" #mandatory
| aws_secret_key = "" #mandatory
| aws_user_arn = "" #mandatory
| aws_role_arn = "" #mandatory
| aws_region = "" #mandatory
| aws_external_id = "" #mandatory
| app_bastion_ssh_key = "" #mandatory
| blk_bastion_ssh_key = "" #mandatory
| app_eks_worker_nodes_ssh_key = "" #mandatory
| blk_eks_worker_nodes_ssh_key = "" #mandatory
| #Cognito specifications
| #When email_sending_account = "COGNITO_DEFAULT", set the below to
  empty in git secrets
| #When email_sending_account = "DEVELOPER", setup verified email
  address in AWS SES on cognito supported region and update the below in
  git secrets
| ses_email_identity = "" #email address verified in AWS SES
| userpool_email_source_arn ="" #arn of the email address configured in
  aws SES service
| #List of iam users and their relevant groups mapping in EKS for its
  access
| #When no additional IAM users are required to enable EKS access, set
  the below as empty in git secrets
| app_cluster_map_users = ["<userarn>","<userarn>"] #Optional, if not
  required set to empty in git secrets
| app_cluster_map_roles = ["<rolearn>","<rolearn>"] #Optional, if not
  required set to emtpy in git secrets
| #List of iam roles and their relevant group mapping in EKS for its
  access
| #When no additional IAM roles are required to enable EKS access, set
  the below as empty in git secrets
| blk_cluster_map_users = ["<userarn>","<userarn>"] #Optional, if not
  required set to empty in git secrets
| blk_cluster_map_roles = ["<rolearn>","<rolearn>"] #Optional, if not
  required set to empty in git secrets
| #Name of S3 bucket to hold terraform input file
| aws_input_bucket = ""
| ################end of sensitive data that goes to git
  secrets#####################
| #set org name as below
| #when nodetype is aais set org_name="aais"
| #when nodetype is analytics set org_name="analytics"
| #when nodetype is aais's dummy carrier set org_name="carrier" and for
  other carriers refer to next line.
| #when nodetype is other carrier set org_name="<carrier_org_name>" ,
  example: org_name = "travelers" etc.,
| org_name = "aais"
| aws_env = "<env>" #set to dev|test|prod

| #--------------------------------------------------------------------------------------------------------------------
| #Application cluster VPC specifications
| app_vpc_cidr = "<app_vpc_cidr>"
| app_availability_zones = ["", ""]
| app_public_subnets = ["", ""]
| app_private_subnets = ["", ""]
| #--------------------------------------------------------------------------------------------------------------------
| #Blockchain cluster VPC specifications
| app_vpc_cidr = "<blk_vpc_cidr>"
| app_availability_zones = ["", ""]
| app_public_subnets = ["", ""]
| app_private_subnets = ["", ""]
| #--------------------------------------------------------------------------------------------------------------------
| #Bastion host specifications
| #Bastion hosts are placed behind nlb. These NLBs can be configured to
  be private \| public to serve SSH traffics.
| #Either case whether NLB is private|public, the source
  ip_address|cidr_block should be enabled in bastion host's security
  group for incoming ssh traffic.
| #in bastion hosts security group for ssh traffic
| #when set to true bastion host's nlb is exposed as public, otherwise
  exposed only to internal to VPC
| bastion_host_nlb_external = "true"
| #application cluster bastion host specifications
| app_bastion_sg_ingress = [
| {rule="ssh-tcp", cidr_blocks = "<app_vpc_cidr>"},
| {rule="ssh-tcp, cidr_blocks = "<cidr_allowed_to_ssh_inbound>"}
| ]
| app_bastion_sg_egress = [
| {rule="https-443-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="http-80-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="ssh-tcp", cidr_blocks = "<app_vpc_cidr>"},
| {rule="ssh-tcp", cidr_blocks = "<cidr_allowed_to_ssh_outbound>"}
| ]
| #blockchain cluster bastion host specifications
| #bastion host security specifications
| blk_bastion_sg_ingress = [
| {rule="ssh-tcp", cidr_blocks = "<blk_vpc_cidr>"},
| {rule="ssh-tcp, cidr_blocks = "<cidr_allowed_to_ssh_inbound>"}
| ]
| blk_bastion_sg_egress = [
| {rule="https-443-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="http-80-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="ssh-tcp", cidr_blocks = "<blk_vpc_cidr>"},
| {rule="ssh-tcp", cidr_blocks = "<cidr_allowed_to_ssh_outbound>"}
| ]
| #--------------------------------------------------------------------------------------------------------------------
| #Route53 (PUBLIC) DNS domain related specifications
| domain_info = {
| r53_public_hosted_zone_required = "<yes>", #Options: yes \| no
| domain_name = "<domain_name>", #primary domain registered
| sub_domain_name = "<sub_domain_name>", #sub domain name
| comments = "<comments>"
| }

| #-------------------------------------------------------------------------------------------------------------------
| #Transit gateway specifications
| tgw_amazon_side_asn = "<amazon_side_asn>" #default is 64532
| #--------------------------------------------------------------------------------------------------------------------
| #Cognito specifications
| userpool_name = "<cognito_pool_name>" #unique user_pool name
| client_app_name = "<cognito_app_client_name>" #a name of the
  application that uses user pool
| client_callback_urls = ["", ""] #ensure to add redirect url part of
  callback urls, as this is required
| client_default_redirect_url = "" #redirect url
| client_logout_urls = [""] #logout url
| cognito_domain = "<cognito_domain_name>" #unique domain name
| # COGNITO_DEFAULT - Uses cognito default. When set to cognito default
  SES related inputs goes empty in git secrets
| # DEVELOPER - Ensure inputs ses_email_identity and
  userpool_email_source_arn are setup in git secrets
| email_sending_account = "COGNITO_DEFAULT" # Options: COGNITO_DEFAULT
  \| DEVELOPER
| #--------------------------------------------------------------------------------------------------------------------
| #Any additional application specific traffic to be allowed in app_vpc
| app_eks_workers_app_sg_ingress = [
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "<blk_vpc_cidr>"
| },
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "<app_vpc_cidr>"
| }]
| app_eks_workers_app_sg_egress = [{rule = "all-all"}]
| #Any additional application specific traffic to be allowed in blk_vpc
| blk_eks_workers_app_sg_ingress = [
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "<blk_vpc_cidr>"
| },
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "<app_vpc_cidr>"
| }]
| blk_eks_workers_app_sg_egress = [{rule = "all-all"}]
| #--------------------------------------------------------------------------------------------------------------------
| # application cluster EKS specifications
| app_cluster_name = "<app_cluster_name>"
| app_cluster_version = "<version>"

app_worker_nodes_ami_id = "AMI-ID"

| #--------------------------------------------------------------------------------------------------------------------
| # blockchain cluster EKS specifications
| blk_cluster_name = "<blk_cluster_name>"
| blk_cluster_version = "<version>"
| blk_worker_nodes_ami_id = "<AMI-ID>"

| #--------------------------------------------------------------------------------------------------------------------
| #cloudtrail related
| cw_logs_retention_period = "<days>" #example 90 days
| s3_bucket_name_cloudtrail = <s3_bucket_name> #s3 bucket name to manage
  cloudtrail logs

| #Name of the S3 bucket managing terraform state files
| terraform_state_s3_bucket_name = "<s3_bucket_aws_resources_pipeline> "

| #Name of the S3 bucket used to store the data extracted from HDS for
  analytics
| #Applicable for carrier and analytics node only. For AAIS node leave
  it empty
| s3_bucket_name_hds_analytics = "<s3_bucket_name_hds_data_analytics>"
| #S3 public bucket to manage application related images (logos)
| s3_bucket_name_logos = "<unique_bucket_name>"

Sample input file used for aais_node setup
------------------------------------------

| org_name = "aais" # For aais set to aais, for analytics set to
  analytics, for carriers set their org name, ex: travelers
| aws_env = "dev" #set to dev|test|prod
| #--------------------------------------------------------------------------------------------------------------------
| #Application cluster VPC specifications
| app_vpc_cidr = "172.26.0.0/16"
| app_availability_zones = ["us-west-2a", "us-west-2b"]
| app_public_subnets = ["172.26.1.0/24", "172.26.2.0/24"]
| app_private_subnets = ["172.26.3.0/24", "172.26.4.0/24"]
| #-------------------------------------------------------------------------------------------------------------------
| #Blockchain cluster VPC specifications
| blk_vpc_cidr = "172.27.0.0/16"
| blk_availability_zones = ["us-west-2a", "us-west-2b"]
| blk_public_subnets = ["172.27.1.0/24", "172.27.2.0/24"]
| blk_private_subnets = ["172.27.3.0/24", "172.27.4.0/24"]
| #--------------------------------------------------------------------------------------------------------------------
| #Bastion host specifications
| #bastion hosts are placed behind nlb. These NLBs can be configured to
  be private \| public to serve SSH.
| #In any case whether the endpoint is private|public for an nlb, the
  source ip_address|cidr_block should be enabled
| #in bastion hosts security group for ssh traffic
| bastion_host_nlb_external = "true"
| #application cluster bastion host specifications
| app_bastion_sg_ingress = [
| {rule="ssh-tcp", cidr_blocks = "172.26.0.0/16"},
| {rule="ssh-tcp", cidr_blocks = "3.237.88.84/32"}]
| app_bastion_sg_egress = [
| {rule="https-443-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="http-80-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="ssh-tcp", cidr_blocks = "172.26.0.0/16"}] #additional
  ip_address|cidr_block should be included for ssh
| #blockchain cluster bastion host specifications
| #bastion host security specifications
| blk_bastion_sg_ingress = [
| {rule="ssh-tcp", cidr_blocks = "172.27.0.0/16"},
| {rule="ssh-tcp", cidr_blocks = "3.237.88.84/32"}]
| blk_bastion_sg_egress = [
| {rule="https-443-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="http-80-tcp", cidr_blocks = "0.0.0.0/0"},
| {rule="ssh-tcp", cidr_blocks = "172.27.0.0/16"}] #additional
  ip_address|cidr_block should be included for ssh
| #--------------------------------------------------------------------------------------------------------------------
| #Route53 (PUBLIC) DNS domain related specifications
| domain_info = {
| r53_public_hosted_zone_required = "yes", #Option: yes \| no
| domain_name = "aaisonline.com", #primary domain registered
| sub_domain_name = "demo", #sub domain
| comments = "aais node dns name resolutions"
| }

| #-------------------------------------------------------------------------------------------------------------------
| #Transit gateway specifications
| tgw_amazon_side_asn = "64532" #default is 64532
| #--------------------------------------------------------------------------------------------------------------------
| #Cognito specifications
| userpool_name = "openidl"
| client_app_name = "openidl-client"
| client_callback_urls =
  ["https://openidl.aais.dev.aaisdemo.com/callback",
  "https://openidl.aais.dev.aaisdemo.com/redirect"]
| client_default_redirect_url =
  "https://openidl.aais.dev.aaisdemo.com/redirect"
| client_logout_urls = ["https://openidl.aais.dev.aaisdemo.com/signout"]
| cognito_domain = "aaisdemo" #unique domain name
| email_sending_account = "COGNITO_DEFAULT" # Options: COGNITO_DEFAULT
  \| DEVELOPER
| # COGNITO_DEFAULT - Uses cognito default and SES related inputs goes
  to empty in git secrets
| # DEVELOPER - Ensure inputs ses_email_identity and
  userpool_email_source_arn are setup in git secrets
| #--------------------------------------------------------------------------------------------------------------------
| #Any additional application specific traffic to be allowed in app_vpc
| app_eks_workers_app_sg_ingress = [
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "172.27.0.0/16"
| },
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "172.26.0.0/16"
| }]
| app_eks_workers_app_sg_egress = [{rule = "all-all"}]
| #Any additional application specific traffic to be allowed in blk_vpc
| blk_eks_workers_app_sg_ingress = [
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "172.27.0.0/16"
| },
| {
| from_port = 443
| to_port = 443
| protocol = "tcp"
| description = "inbound https traffic"
| cidr_blocks = "172.26.0.0/16"
| }]
| blk_eks_workers_app_sg_egress = [{rule = "all-all"}]
| #--------------------------------------------------------------------------------------------------------------------
| # application cluster EKS specifications
| app_cluster_name = "app-cluster"
| app_cluster_version = "1.20"
| app_worker_nodes_ami_id = "ami-06f175a2687bd1c1e"

| #--------------------------------------------------------------------------------------------------------------------
| # blockchain cluster EKS specifications
| blk_cluster_name = "blk-cluster"
| blk_cluster_version = "1.20"
| blk_worker_nodes_ami_id = "ami-06f175a2687bd1c1e"

| #--------------------------------------------------------------------------------------------------------------------
| #cloudtrail related
| cw_logs_retention_period = 90
| s3_bucket_name_cloudtrail = "cloudtrail-logs"

| #Name of the S3 bucket managing terraform state files
| terraform_state_s3_bucket_name = "aais-test-tfstate-mgmt"

| #Name of the S3 bucket used to store the data extracted from HDS for
  analytics
| #Applicable for carrier and analytics node only. For AAIS node leave
  it empty
| s3_bucket_name_hds_analytics = ""
| #S3 public bucket to manage application related images (logos)
| s3_bucket_name_logos = "openidl-logos"
