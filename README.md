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
ansible-playbook jenkins-instance/jenkins-instance-setup.yml --extra-vars '{"instance": "", "ami": "ami-0bc42be5271f1585a", "EIP": "Your_Elastic_IP"}' -vvv

ansible-playbook jenkins-instance/jenkins-instance-stop.yml -vvv

ansible-playbook jenkins-instance/jenkins-instance-terminate.yml --extra-vars '{"input_filters": {"tag:app": "jenkins", "instance-state-name": "running"}, "EIP": "Your_Elastic_IP"}' -vvv
```

## Certbot Certificate
Variables:

  - `instance`: should be either `atlantis` or `jenkins`

```bash
ansible-playbook atlantis-instance/atlantis-instance-certificate-setup.yml --extra-vars '{"instance": "", "domain_name": "xxxxx.xxxx.me", "letsencrypt_email": "xxxx@gmail.com"}' -vvv
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