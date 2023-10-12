# aap-containerized-demo
Repo to experiment with Containerized Ansible Automation Platform

I do this from my Mac terminal, currently runing ansible-core 2.14.1 
Download the installer and update vars/config.yml to point at it.  I just 
dumped the installer in my repo directory (it's in the .gitignore).  

You'll need an extra_vars file like this:

```yaml
---
redhat_username: < you username here >
redhat_password: < your password here >

admin_password: < set AAP Admin password >

certificate_env: staging # staging or prod, only switch to prod when you've confirmed your setup works
cert_email: < your email >
# fair warning if you fail to use the included teardown playbook, you'll get emails as those certs expire in 90 days

# Don't set this as true unless something horrible has happened and you think it's cert related
certificate_force: false 
```

and a Red Hat Demo System AWS OpenEnvironment credential setup as a default profile (or update vars/config.yml with your preferred profile name) at `~/.aws/credential`

To run the setup 
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml setup.yml `

To run the teardown
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml teardown.yml`

# To Do 
- Create an update security rules playbook with the role
- Create infra.controller_configuration config files so we can use the controller postinstall options.