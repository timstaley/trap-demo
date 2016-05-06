# TraP-Demo
A relatively simple means of setting up [TraP][] in a [virtual machine][] so you
can easily try it out without making changes to your local machine,
whether you run Linux, Mac OSX, or (possibly) even Windows.

[TraP]: http://tkp.readthedocs.io/en/latest/introduction.html
[virtual machine]: https://simple.wikipedia.org/wiki/Virtual_machine

## Setup
### Pre-requisites:

You will need:

- [Virtualbox](https://www.virtualbox.org/wiki/Downloads). This provides the
    virtual machine container. When this is successfully installed you
    should be able to launch the Virtualbox GUI from start menu
    (just to check it's working OK - we won't actually be using the GUI).
- [Vagrant](https://www.vagrantup.com/downloads.html).
  Vagrant provides a convenient command line interface for setting up and
  configuring virtual machines. You can check if vagrant is installed OK by
  running `vagrant help` from the command line.
- [Ansible](http://docs.ansible.com/ansible/intro_installation.html). Ansible
  provides a scripting language and associated tools for system installation
  and configuration. It's a Python package, so my preferred installation method
  is to simply install it using `pip`, but see the Ansible docs for other
  options. You can check if ansible is installed and in your path by running
  `ansible --help` from the command line.

### Installation:

From the command line:

Clone this repository and navigate to the *vagrant* directory:

    git clone https://github.com/timstaley/trap-demo.git
    cd trap-demo/vagrant

Next, install the Ansible [roles][] - these are 'building blocks' for system
configuration, e.g. we use one role to install [casacore][], another to configure
a [PostgreSQL][] database with the desired user-permissions, etc.
The required roles are listed in *requirements.txt*,
and we can install them with:

    ansible-galaxy install -r requirements.txt

[roles]: http://docs.ansible.com/ansible/playbooks_roles.html#roles
[casacore]: https://github.com/casacore/casacore
[PostgreSQL]: http://www.postgresql.org/about/

We're ready to go! The *Vagrantfile* is configured so that vagrant will
create a virtual machine running Ubuntu 14.04 and then install TraP and Banana
using the Ansible configuration scripts. Simply run:

    vagrant up

(from the *vagrant* directory, where the *Vagrantfile* is located).
This will take a while the first time - perhaps
an hour or so depending on your internet connection speed, etc.
If something goes wrong (maybe you get disconnected from the
internet or whatever) then you can resume the install process using:

    vagrant provision

That's also the command to run if you change the configuration scripts and want
to bring your virtual machine up to date.

When everything's up and running, try bringing up a web-browser and take a look
at [http://localhost:8080](http://localhost:8080) - if everything worked OK you
should see the front page of the Banana web-interface.

You can also connect to your virtual machine over SSH. Try:

    vagrant ssh

You should be greeted by the login message and terminal prompt on your virtual
machine.

### Teardown:
Running a virtual machine uses up some of your RAM, and some hard drive space.
If you want to 'switch off' the virtual machine and free up your RAM again, run:

    vagrant halt

(you can `vagrant up` later to switch it back on again).
If you want to delete the virtual machine entirely and reclaim your disk space,
run:

    vagrant destroy

## Data reduction
The next step is to actually analyse some radio-astronomy images using the TraP.
We'll refer to the virtual machine as the 'guest machine' or just 'the guest'
from here on. (Your regular operating system is the 'host'.)

Vagrant configures the guest machine so that the *vagrant* folder we've been
working from (on the host) is also visible from the guest, at the path
*/vagrant*. If you `vagrant ssh` into the guest and then `cd /vagrant` you can
check this for yourself. The setup scripts created a couple of subfolders here,
*trap-jobs* and *trap-data*, which contain a ready-made example of a job
configuration and sample data, respectively.
