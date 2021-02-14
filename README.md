# infra-ansible
Repository for Ansible Playbooks

## Instructions for using packer

- [packer docs](https://learn.hashicorp.com/collections/packer/getting-started)
- Install packer on Linux:
  - `curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -`
  - `sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"`
  - `sudo apt-get update && sudo apt-get install packer`
- check the version
  - `packer -v`
- validate
  - `packer validate example.json`
- build
  - packer build \
      -var 'aws_access_key=YOUR ACCESS KEY' \
      -var 'aws_secret_key=YOUR SECRET KEY' \
      -var 'aws_region=us-east-1' \
      -var 'subnet_id=${{ secrets.AWS_SUBNET_ID }}' \
      -var 'ami_users=${{ secrets.AMI_USERS }}' \
      example.json