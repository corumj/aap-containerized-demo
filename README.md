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

and also an ansible.cfg file configured for Automation Hub, something like this:
```ini
[defaults]
host_key_checking = False
# callback_whitelist = profile_tasks
deprecation_warnings=False
[galaxy]
server_list = automation_hub, galaxy 

[galaxy_server.automation_hub]
url= < Server URL from Automation Hub - Connect to Hub >
auth_url= < SSO url from Automation Hub as well >
token=< token  goes here, generated, you guessed it, from Automation Hub >

[galaxy_server.galaxy]
url=https://galaxy.ansible.com
```

To run the setup 
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml setup.yml `

To shut down AAP so that you can come back tomorrow, run:
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml shutdown.yml`

To start up AAP and resume where you left off from yesterday, run:
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml startup.yml`

To teardown the instances, run:
`ansible-playbook -i aws_ec2.yml -e @extra_vars.yml teardown.yml`
> [!WARNING]
> Remember to always run this when you are completely finished with
> the environment, otherwise your certs will expire 
> and you'll get tons of emails from Let's Encrypt about it.

# To Do 
- [ ] Create an update security rules playbook with the role
- [-] Create infra.controller_configuration config files so we can use the controller postinstall options. (in progress)
- [ ] Fully parameterize the remaining config.yml keys with paths
- [ ] Clean up with module_defaults
- [ ] See if the dns role can be modified to handle a situation with multiple zones (maybe by looking for the controller record? )