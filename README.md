# infra-ansible

Repository for Ansible Playbooks

## Networking

Variables:

  - `service_name`: should be either `atlantis` or `jenkins`

Examples:

```
ansible-playbook networking/networking-setup.yml --extra-vars '{"service_name": ""}' -vvv

ansible-playbook networking/networking-terminate.yml --extra-vars '{"service_name": ""}' -vvv
```

## Jenkins Instance
Variables:

  - `instance`: should be either `atlantis` or `jenkins`

```bash
ansible-playbook instance/instance-setup.yml --extra-vars '{"instance": "jenkins", "ami": "ami-0a19ced6bd18d0b30", "EIP": "18.214.21.105", "aws_hosted_zone": "bh7cw.me", "aws_dns_record": "jenkins.bh7cw.me"}' -vvv
ansible-playbook instance/instance-terminate.yml --extra-vars '{"instance": "jenkins", "EIP": "18.214.21.105", "aws_hosted_zone": "bh7cw.me", "aws_dns_record": "jenkins.bh7cw.me"}' -vvv

ansible-playbook instance/instance-setup.yml --extra-vars '{"instance": "atlantis", "ami": "ami-0b64603de4eef463c", "EIP": "54.234.163.235", "aws_hosted_zone": "bh7cw.me", "aws_dns_record": "atlantis11.bh7cw.me"}' -vvv
ansible-playbook instance/instance-terminate.yml --extra-vars '{"instance": "atlantis", "EIP": "54.234.163.235", "aws_hosted_zone": "bh7cw.me", "aws_dns_record": "atlantis11.bh7cw.me"}' -vvv
```

## Certbot Certificate
Variables:

  - `instance`: should be either `atlantis` or `jenkins`

```bash
ansible-playbook instance/instance-certificate-setup.yml --extra-vars '{"instance": "jenkins", "domain_name": "jenkins.bh7cw.me", "letsencrypt_email": "ibh7cw@gmail.com"}' -vvv

ansible-playbook instance/instance-certificate-setup.yml --extra-vars '{"domain_name": "atlantis11.bh7cw.me", "letsencrypt_email": "ibh7cw@gmail.com"}' -vvv
```

## Atlantis Service

```bash
ansible-playbook instance/atlantis-service-start.yml --extra-vars '{"config_file_path": "templates/config.j2"}' -vvv
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