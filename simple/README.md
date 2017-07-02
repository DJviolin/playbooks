# Ansible test

#### DigitalOcean integration

https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2-with-ansible-2-0-on-ubuntu-16-04

https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2-with-ansible-2-0-on-ubuntu-14-04

https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-16-04

https://www.digitalocean.com/community/tags/ansible?type=tutorials

#### Download fix for Windows:

https://bintray.com/artifact/download/vszakats/generic/curl-7.54.1-win64-mingw.7z

#### Fix the "mount: unknown filesystem type 'vboxsf'" error:

```bash
$ vagrant plugin install vagrant-vbguest
$ vagrant destroy && vagrant up
```

#### Vagrant init:

```bash
$ vagrant init debian/stretch64
$ vagrant up
$ vagrant halt
$ vagrant status
$ vagrant ssh-config
$ vagrant reload
```

#### Vagrant dance:

```bash
$ vagrant destroy -f && vagrant up
```

#### SSH into one machine

```bash
$ vagrant ssh master
```

#### Verifying network adapter's IP

```bash
$ ip addr show eth1 | grep inet
```

#### Verify that ansible can connect to the server:

```bash
$ ssh vagrant@192.168.50.11 -p 22 -i /vagrant-machines/node1/virtualbox/private_key
$ ansible testserver -i /vagrant/shared/hosts -m ping -vvvv
# After simplify:
$ cd /ansible
$ ansible testserver -m ping
# Executing commands:
$ ansible testserver -m command -a uptime
# The command module is so commonly used that it’s the default module,
# so we can omit it:
$ ansible testserver -a uptime
$ ansible testserver -a "tail /var/log/dmesg"
# If we need root access:
$ ansible testserver -s -a "tail /var/log/syslog"
# We can install packages with this command also:
$ ansible testserver -s -m apt -a name=nginx
# You might also need to update packages than:
$ ansible testserver -s -m apt -a "name=nginx update_cache=yes"
# Restart nginx:
$ ansible testserver -s -m service -a "name=nginx state=restarted"
# We need the -s argument to use sudo because only root can install
# the nginx package and restart services.

# Show network address
$ ansible node2 -a "ip addr show dev eth1"
# Show free memory
$ ansible webservers -a "free -m" --forks=1
# Output everything that Ansible knows about the server
$ ansible webservers -m setup --forks=1
# Piping output
$ ansible webservers -m shell -a "ip addr show | grep inet"
# Restart Nginx service
$ ansible webservers -m service -a "name=nginx state=restarted" -s --forks=1

# Execute on all hosts
$ ansible all -a "date"
$ ansible '*' -a "date"
```

#### Running the Playbook

```bash
$ ansible webservers -m ping
$ ansible-playbook web-notls.yml
# or
$ ./web-notls.yml
```

#### Viewing Ansible Doc

```bash
$ ansible-doc service
```

#### Genereate TLS certificate

```bash
$ openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -subj /CN=localhost -keyout files/nginx.key -out files/nginx.crt
# then run the config again
$ ./web-tls.yml
```

#### Dynamic inventory (automatic host tracking)

```bash
$ pip2 install paramiko
$ c:\python27\python.exe inventory/vagrant.py --list
$ c:\python27\python.exe inventory/vagrant.py --host=node1
```

#### Viewing All Facts Associated with a Server

```bash
$ ansible node1 -m setup
# Filtering only relevant information
$ ansible node1 -m setup -a 'filter=ansible_eth*'
```

Any Module Can Return Facts

```yaml
- name: get ec2 facts
  ec2_facts:
- debug: var=ansible_ec2_instance_id
```

Returns:

```bash
TASK: [debug var=ansible_ec2_instance_id] *************************************
ok: [myserver] => {
"ansible_ec2_instance_id": "i-a3a2f866"
}
```

#### CLI arguments for playbooks

```bash
$ ./greet.yml -e greeting=hiya
$ ansible-playbook greet.yml -e 'greeting="hi there"'
# Ansible also allows you to pass a file containing the variables
# instead of passing them directly
$ ./greet.yml -e @greetvars.yml
```

#### Listing tasks of a playbook

```bash
$ ansible-playbook --list-tasks mezzanine/mezzanine.yml
# https://github.com/stephenmcd/mezzanine/blob/master/mezzanine/project_template/fabfile.py
```
