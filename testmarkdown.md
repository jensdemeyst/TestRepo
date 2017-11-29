# Enterprise Linux Lab Report

- Student name: Detemmerman Thomas
- Github repo: <https://github.com/HoGentTIN/elnx-sme-ThomasDetemmerman>

Create a lamp server supporting HTTPS. The firewall and SELinux should be active. Also a working certificate must be present on the server.


## Test plan

- On the host system, go to the local working directory of the project repository
- Execute `vagrant status`
    - There should be one VM, `pu004` with status `not created`. If the VM does exist, destroy it first with `vagrant destroy -f pu004`
- Execute `vagrant up pu004`
    - The command should run without errors (exit status 0)
- Log in on the server with `vagrant ssh pu004` and run the acceptance tests `sudo sh /vagrant/test/runbats.sh`.
    - They should succeed
- Browse from your host to `192.0.2.50/wordpress/`
    - This should return the wordpress page
- Browse from you host to  `https://192.0.2.50/wordpress/`
    -  Also this should return the wordpress page
- Check from your host if the SSL certificate is present

## Procedure/Documentation
### Prerequirements
	- virtualization software (like VirtualBox)
	- vagrant
	- ansible
	- git
### Download files
open git and run `git clone git@github.com:HoGentTIN/elnx-sme-ThomasDetemmerman.git`
<br><b>note:</b> It is important to download the repository over ssh since https can cause problems with the test-script.

### Set up the environment
Go to the location where you downloaded the repository and simply run in git `vagrant up`

### Documentation
Creating a Lamp-stack wasn't the hard part since it isn't the first time we have to set up such stack. Making it possible to create
a connection over HTTPS was someting more challanging.
- Since this was the first time and I didn't know how to create a SSL-certificate, I followed the steps described in the 'wiki how to' (CentOS - HowTos).
- I decided to create a SSL-certificate directly on the VM, just to try out how it worked. To make it automated was something to do afterwards.
- After destorying pu004, these files would be lost. So they must be copied from the host to the guest.
- for this to make it work, I created a new key and crt on the host using git bash.
- after having checked the ansible documentation, the module copy seemed to be appropriate for this task.
- The demo of mr. Van Vreckem added the mission part of  `pre-task`
- According to the Ansible-role, two variables (for key and crt) needed  to be defined, describing the location of the corresponding files.

## Test report


- Execute `vagrant status`
	<br>✓ <b>Succesvol</b><br>
```bash
Gebruiker@Thomas MINGW64 ~/OneDrive/3 Tin - Semester 1/Linux Enterprise/elnx-sme-ThomasDetemmerman-byGitSSH (master)
$ vagrant status
Current machine states:

pu004                     not created (virtualbox)

The environment has not been created yet. Run `vagrant up` to
create the environment. If a machine is not created, only the
default provider will be shown. So if a provider is not listed,
then the machine is not created for that environment.
```


- Execute `vagrant up pu004`
    <br>✓ <b>Succesvol</b><br>

```bash
==> pu004: pu004                      : ok=51   changed=38   unreachable=0    failed=0

```

- Log in on the server with `vagrant ssh pu004` and run the acceptance tests. They should succeed
<br>✓ <b>Succesvol</b><br>

```bash
$ vagrant ssh pu004
Welcome to pu004..
enp0s3     : 10.0.2.15         fe80::a00:27ff:febb:91b7/64
enp0s8     : 192.0.2.50        fe80::a00:27ff:fe0a:7d80/64
[vagrant@pu004 ~]$ sudo sh /vagrant/test/runbats.sh
--2016-11-14 21:47:52--  https://github.com/sstephenson/bats/archive/v0.4.0.tar.                    gz
Resolving github.com (github.com)... 192.30.253.112, 192.30.253.113
Connecting to github.com (github.com)|192.30.253.112|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://codeload.github.com/sstephenson/bats/tar.gz/v0.4.0 [following]
--2016-11-14 21:47:52--  https://codeload.github.com/sstephenson/bats/tar.gz/v0.                    4.0
Resolving codeload.github.com (codeload.github.com)... 192.30.253.121, 192.30.25                    3.120
Connecting to codeload.github.com (codeload.github.com)|192.30.253.121|:443... c                    onnected.
HTTP request sent, awaiting response... 200 OK
Length: 17258 (17K) [application/x-gzip]
Saving to: ‘v0.4.0.tar.gz’

100%[======================================>] 17,258      --.-K/s   in 0.1s

2016-11-14 21:47:53 (159 KB/s) - ‘v0.4.0.tar.gz’ saved [17258/17258]

Running test /vagrant/test/common.bats
 ✓ EPEL repository should be available
 ✓ Bash-completion should have been installed
 ✓ bind-utils should have been installed
 ✓ Git should have been installed
 ✓ Nano should have been installed
 ✓ Tree should have been installed
 ✓ Vim-enhanced should have been installed
 ✓ Wget should have been installed
 ✓ Admin user thomas should exist
 ✓ Custom /etc/motd should have been installed

10 tests, 0 failures
Running test /vagrant/test/pu004/lamp.bats
 ✓ The necessary packages should be installed
 ✓ The Apache service should be running
 ✓ The Apache service should be started at boot
 ✓ The MariaDB service should be running
 ✓ The MariaDB service should be started at boot
 ✓ The SELinux status should be ‘enforcing’
 ✓ Web traffic should pass through the firewall
 ✓ Mariadb should have a database for Wordpress
 ✓ The MariaDB user should have "write access" to the database
 ✓ The website should be accessible through HTTP
 ✓ The website should be accessible through HTTPS
 ✓ The certificate should not be the default onesit
 ✓ The Wordpress install page should be visible under http://192.0.2.50/wordpress/
 ✓ MariaDB should not have a test database
 ✓ MariaDB should not have anonymous users

15 tests, 0 failures
[vagrant@pu004 ~]$

```
- Browse from your host to `192.0.2.50/wordpress/`
<br>✓ <b>Succesvol</b><br>
 ![site bereikbaar](./pictures/siteAccessible.png)
- Browse from you host to  `https://192.0.2.50/wordpress/`
<br>✓ <b>Succesvol</b><br>
 ![https site bereikbaar](./pictures/httpsSiteAccessible.png)
- Check from your host if the SSL certificate present is <br>
✓ <b>Succesvol</b><br>
 ![SSL certificaat](./pictures/SSLCertificaat.png)





## Resources

<https://github.com/bertvv/ansible-role-rh-base/blob/master/README.md> <br>
<https://wiki.centos.org/HowTos/Https><br>
<http://docs.ansible.com/ansible/copy_module.html>
