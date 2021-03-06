[![Travis CI](https://img.shields.io/travis/inofix/ansible-acme-tiny-sign.svg?style=flat)](http://travis-ci.org/inofix/ansible-acme-tiny-sign)


Acme-Tiny Sign
==============

This is an ansible role for getting digital certificates with "Let's Encrypt". It is highly influenced by this role: ganto.acme\_tiny. Many thanks [ganto](https://linuxmonk.ch/)!

The role is meant to be run for a system accessible from the web. It will make the request with "Let's Encrypt" from an existing csr (see inofix.acme-request), solve the challenge on the server's well-known webfolder and then put the resulting certificates in the openssl configuration directory.

The two other roles, inofix.acme-tiny-install and inofix.acme-tiny-request,
are required. The latter might be run on yet another remote host as to generate the private key and the cert-request.

Why we do not use one of the existing roles?

* For the first reason read the section "Promise" below. We need something reliable.
* This role does not connect to the web as root, but as an unpriviledged user
* This role does not expose the private key file to the unpriviledged acme user
* This cert-request might be done on a remote machine via inofix.acme-tiny-setup such that the private key is not even on the host requesting the certificate.
* This role will be used by [maestro](https://github.com/inofix/maestro) and must follow the logic used there. (Of course, the role can be used without maestro..)


Status
------

preSTABLE (Feature-Freeze/RC)

Promise
-------

Sure, this role may change in the future, but we will only expand features to not break backwards compatibility.

If radical changes should become necessary, a new role will be created, probably with an 'ng' or version suffix...

Installation
------------

 # ansible-galaxy install inofix.acme-tiny-sign

Requirements
------------

* Ansible >2.0
* On target host
 * Generic UNIX with FHS
 * Python2/3
 * OpenSSL
 * Sudo
 * Running webserver with ^/.well-known/acme-challenge/ directory accessible (hardcoded in acme-tiny script..)
 * The webserver must serve HTTP, even for consequent certificates (not just HTTPS; requirement of acme-tiny/let's-encrypt)
 * Resolve all names in the cert to localhost or the local-IP

Role Variables
--------------

* app\_\_acme\_\_user - optional, default='acme'
* app\_\_acme\_\_group - optional, default='acme'
* app\_\_acme\_\_os\_\_cert\_group - optional, default='{{ default\_\_acme\_\_group }}'
* app\_\_acme\_\_config\_dir - optional, default='/etc/ssl/acme'
* app\_\_acme\_\_account\_key - optional, auto
* app\_\_acme\_\_challenge\_dir - optional, default='/var/www/acme-challenge'
* app\_\_acme\_\_domain - optional, default=[ {domain='example.com'} ]
* fqdn - optional, default={{ ansible\_fqdn | d(inventory\_hostname ) }}

Dependencies
------------

* inofix.acme-tiny-install
 * (inofix.acme-setup)
* inofix.acme-request
 * (inofix.acme-setup)

Example Playbook
----------------

    - hosts: servers
      roles:
         - inofix.acme-tiny-sign

(See inofix.acme-setup)


License
-------

GPLv3

Author Information
------------------

* Michael Lustenberger at [inofix.ch](http://www.inofix.ch)
