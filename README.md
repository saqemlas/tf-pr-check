# Terraform Github Action PR Bot

## Information

Using terraform, AWS resources can be created such as IAM roles and Github resources such as repository pipelines. IdP-authenticated pipelines will assume the IAM role with the permissions from the policy document. Pipelines will then have workflows configured with short-lived access token directly from the cloud provider, allowing fine grained of only that pipeline either from main, any pull request, or a specific branch. 

- [Github Actions OpenID Connect with AWS](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services)
- [Creating OpenID Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)
- [Terraform Github Provider](https://registry.terraform.io/providers/integrations/github/latest/docs)


## Environment

This project uses...
- [terraform](https://learn.hashicorp.com/tutorials/terraform/infrastructure-as-code?in=terraform/aws-get-started) (v1.4.6) For IaC.

### Usage:

This github action expects terraform code within a `tf/` directory and also requires secrets set;
- `TF_VERSION` - terraform version used
- `PIPELINE_ROLE_ARN` - oidc provider IAM role, allowing the pipeline to deploy into an AWS environment
- `OWNER_GITHUB` - githu owner of the repository
- `TOKEN_GITHUB` - github token with repo permissions, allowing to create repository secrets

When a new pull request is created, the pipeline (see `.github/workflows/pull_request.yml`) validates and plans the terraform state. It then creates a comment on the pull request with the output of the state file.

On merge to main, the pipeline (see `.github/workflows/deploy.yml`) validates, plans, and applies the terraform state on the AWS environment.
