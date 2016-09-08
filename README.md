# ansible-role-jenkins

This role is meant to install a very specific version of Jenkins. 

* Ubuntu Xenial 16.04
* Self-signed certificate
* https only on port 8443, no http
* Install plugins automatically
* Keep it as simple as possible

If that is what you are looking for, or would like to base your role on this, then great! If not, then you could probably use this as a starting point.

In most production deployments you would have your own TLS certificates and probably put Apache or Haproxy or something else in front of Jenkins to terminate TLS for you.

## Options

The only real option is what other plugins you would like. There is a *jenkins_plugins_custom* variable that is expected to be an array of plugins. If that is set then those custom plugins will be installed while the recommended plugins are being installed.

## Admin password

Jenkins 2+ seems to create an initial Admin password.

It's located in */var/lib/jenkins/secrets/initialAdminPassword*. That password is also set as a fact in this role, and it's assigned to the fact *jenkins_admin_password*.

## Example usage

```
- hosts: jenkins
  gather_facts: False
  tasks:
    # Sometimes Xenial doesn't have python 2.7
    - name: ensure python 2.7 is installed
      raw: apt-get install -y python2.7 python-simplejson

- hosts: jenkins
  vars:
    # Install the ansible jenkins plugin too
    - jenkins_plugins_custom:
        - ansible
  roles:
    - jenkins
```

## Thanks

Thanks to [geerlingguy](https://github.com/geerlingguy) for his [Jenkins role](https://github.com/geerlingguy/ansible-role-jenkins) for inspiration.
