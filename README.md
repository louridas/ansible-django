A Simple Ansible Playbook for Django
====================================

This project contains the simplest possible
[ansible](http://docs.ansible.com/ansible) playbook to provision a server for a
Django project and deploy new versions of the project.

It has been forked from [technivore/ansible-django](https://github.com/technivore/ansible-django), 
but several changes were made so that:
  * It works with MySQL, instead of PostgreSQL.
  * It works with Ubuntu 16.04 servere, instead of Ubuntu 14.04 server. That means that it
  uses `systemd` instead of `upstart` for the service configuration.

### Assumptions

This playbook assumes you have created an Ubuntu 16.04 server and have already
set up ssh public key authentication on it. 

### How to use

- Install ansible on your computer (`sudo pip install ansible` should do the
  trick).
- Clone this project.
- Edit the `ansible/hosts` file and enter the IP address of the server you are
	deploying to and edit the path to the private key file.
- Edit the `ansible/vars.yml` file and enter values appropriate for your project.
- Run the playbook:

	```
	cd ansible
	ansible-playbook -i hosts provision.yml
	```	

That's it! Your project is now up and running and assuming you've set up a DNS
record for your server, you can now visit your site.

As you make changes and commit to your django project you'll obviously want to
push the new version to production, so to do that without having to run the
full playbook, you can run this command (from the `ansible` directory):
`ansible-playbook -i hosts deploy.yml`

That will check out the latest version of your code, install any new
dependencies via pip if necessary, run the `migrate` and `collectstatic` django
management commands, and restart nginx and gunicorn if necessary.
