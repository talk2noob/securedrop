Getting Started
===============

Prerequisites
-------------

SecureDrop is a multi-machine design. To make development and testing easy, we
provide a set of virtual environments, each tailored for a specific type of
development task. We use Vagrant and VirtualBox to conveniently develop with a
set of virtual environments, and our Ansible playbooks can provison these
environments on either virtual machines or physical hardware.

To get started, you will need to install Vagrant, VirtualBox, and Ansible on
your development workstation.


Ubuntu/Debian
~~~~~~~~~~~~~

.. note:: Tested on: Ubuntu 14.04 and Debian Stretch

.. code:: sh

   sudo apt-get install -y dpkg-dev virtualbox-dkms linux-headers-$(uname -r) build-essential git

We recommend using the latest stable version of Vagrant (``1.8.5`` at the time
of this writing), which might be newer than what is in your distro's package
repositories.

If ``apt-cache policy vagrant`` says your candidate version is not at least 1.7,
you should download the current version from the `Vagrant Downloads page`_ and
then install it.

.. code:: sh

    # If you downloaded vagrant.deb from vagrantup.com
    sudo dpkg -i vagrant.deb
    # OR this, if your OS vagrant is recent enough
    sudo apt-get install vagrant

We *do not* recommend using a version of Vagrant older than 1.8.4. For instance,
the version of Vagrant currently in the Ubuntu Trusty repositories is 1.5.4,
which is signficantly out of date and known not to work with SecureDrop (`GitHub
#932`_, `GitHub #1381`_).

.. _`Vagrant Downloads page`: https://www.vagrantup.com/downloads.html
.. _`GitHub #932`: https://github.com/freedomofpress/securedrop/pull/932
.. _`GitHub #1381`: https://github.com/freedomofpress/securedrop/issues/1381

.. warning:: We do not recommend installing vagrant-cachier. It destroys apt’s
            state unless the VMs are always shut down/rebooted with Vagrant,
            which conflicts with the tasks in the Ansible playbooks. The
            instructions in Vagrantfile that would enable vagrant-cachier are
            currently commented out.

.. todo:: This warning is here because a common refrain during hackathons for
          SecureDrop a while back was "setting up VMs is too slow, you should
          use vagrant-cachier". We tried it and it had some nasty interactions
          with Ansible, so we dropped it, and added this note to prevent other
          people from making the same suggestion. Eventually, we should: (i)
          Build our own base boxes to dramatically cut down on provisioning
          times (ii) Remove this note as well as the commented vagrant-cachier
          lines from the Vagrantfile

Either way, once you've installed Vagrant you should run:

.. code:: sh

    sudo dpkg-reconfigure virtualbox-dkms

Finally, install Ansible so it can be used with Vagrant to automatically
provision VMs. We recommend installing Ansible from PyPi with ``pip`` to ensure
you have the latest stable version.

.. code:: sh

    sudo apt-get install python-pip

The version of Ansible recommended to provision SecureDrop VMs may not be the
same as the version in your distro's repos, or may at some point flux out of
sync. For this reason, and also just as a good general development practice, we
recommend using a Python virtual environment to install version 1.8.4 of
Ansible. We provide instructions using `virtualenvwrapper
<http://virtualenvwrapper.readthedocs.io/en/stable/>`_ and `virtualenv
<https://virtualenv.readthedocs.io/en/latest/>`_.

Using virtualenvwrapper:

.. code:: sh

    sudo apt-get install virtualenvwrapper
    mkvirtualenv -p python2.7 securedrop
    pip install ansible==1.8.4

Using virtualenv (we recommend you `cd` into the  base directory of the repo
before running these commands):

.. code:: sh

    sudo apt-get install virtualenv
    virtualenv -p python2.7 .
    . bin/activate
    pip install ansible==1.8.4

Mac OS X
~~~~~~~~

Install the dependencies for the development environment:

#. Vagrant_
#. VirtualBox_
#. Ansible_.

   There are several ways to install Ansible on a Mac. We recommend installing
   from PyPi using ``pip`` so you will get the latest stable version:

   .. code:: sh

      sudo easy_install pip && sudo pip install -U ansible

.. _Vagrant: http://www.vagrantup.com/downloads.html
.. _VirtualBox: https://www.virtualbox.org/wiki/Downloads
.. _Ansible: http://docs.ansible.com/intro_installation.html

Clone the repository
--------------------

Once you've installed the prerequisites for the development environment,
use git to clone the SecureDrop repo.

.. code:: sh

   git clone https://github.com/freedomofpress/securedrop.git

SecureDrop uses a branching model based on `git-flow
<http://nvie.com/posts/a-successful-git-branching-model/>`__.  The ``master``
branch always points to the latest stable release. Use this branch if you are
interested in installing or auditing SecureDrop.  Development for the upcoming
release of SecureDrop takes place on ``develop``, which is the default
branch. If you want to contribute, you should branch from and submit pull
requests to ``develop``.

.. todo:: The branching model should be documented separately, in a
	  "Contributing guidelines" document. We are also going to move away
	  from git-flow soon because it sucks.
