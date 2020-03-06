# packer-workflows
A packer project to build an AMI containing software for running bioinformatics workflows

## Development

### Setup
* Install [packer](https://www.packer.io/intro/getting-started/install.html)
* Install [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

### Validate
Choose an ImageName such as "MyImage" and run
```
packer validate -var 'ImageName=MyImage' template.json
```

### Manual AMI Build
If you would like to test building an AMI run:
```
packer build -var AwsProfile=my-aws-account -var AwsRegion=us-east-1 src/template.json
```

Packer will do the following:
* Create a temporary EC2 instance, configure it with shell/ansible/puppet/etc. scripts.
* Create an AMI from the EC2
* Delete the EC2

__Note__: Packer deploys a new AMI to the AWS account specified by the AwsProfile

### Pull Requests and Versions.
To make changes, we create pull requests.
Once these changes are merged to master, create a tag.
When the tag is pushed travis will build an AMI version with that tag number.

## Contributions
Contributions are welcome.

Requirements:
* Install [pre-commit](https://pre-commit.com/#install) app
* Clone this repo
* Run `pre-commit install` to install the git hook.

## Testing
As a pre-deployment step we syntatically validate our packer json
files with [pre-commit](https://pre-commit.com).

Please install pre-commit, once installed the file validations will
automatically run on every commit.  Alternatively you can manually
execute the validations by running `pre-commit run --all-files`.

## Deployments
Travis runs packer which temporarily deploys an EC2 to create an AMI.

## Continuous Integration
We have configured Travis to deploy updates.

## Issues
* https://sagebionetworks.jira.com/projects/IT

## Builds
* https://travis-ci.org/Sage-Bionetworks/packer-workflows

## Secrets
* We use the [AWS SSM](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-paramstore.html)
to store secrets for this project.  Sceptre retrieves the secrets using
a [sceptre ssm resolver](https://github.com/cloudreach/sceptre/tree/v1/contrib/ssm-resolver)
and passes them to the cloudformation stack on deployment.
