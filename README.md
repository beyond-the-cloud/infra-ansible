# infra-ansible

Repository for Ansible Playbooks

## Networking

Variables:

  - `service_name`: should be either `atlantis` or `jenkins`

Examples:

```bash
ansible-playbook networking/networking-setup.yml --extra-vars '{"service_name": ""}' -vvv

ansible-playbook networking/networking-teardown.yml --extra-vars '{"service_name": ""}' -vvv
```

## Jenkins Instance
Variables:

  - `instance`: should be either `atlantis` or `jenkins`

```bash
ansible-playbook instance/instance-setup.yml --extra-vars '{"instance": "", "ami": "", "EIP": "", "aws_hosted_zone": "xxxx.me", "aws_dns_record": "xxxx.xxxx.me"}' -vvv

ansible-playbook instance/instance-terminate.yml --extra-vars '{"instance": "", "EIP": "", "aws_hosted_zone": "xxxx.me", "aws_dns_record": "xxxx.xxxx.me"}' -vvv
```

## Certbot Certificate

```bash
ansible-playbook instance/instance-certificate-setup.yml --extra-vars '{"instance": "", "domain_name": "xxxx.xxxx.me", "letsencrypt_email": "xxxx@gmail.com"}' -vvv
```

## Atlantis Service

```bash
ansible-playbook instance/atlantis-service-start.yml --extra-vars '{"config_file_path": "to/path/config.j2"}' -vvv
```

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