When building an AIO on a new server, it is recommended that all
system packages are upgraded and then reboot into the new kernel:

   .. code-block:: shell-session

       root@ubuntu14.04:~# apt-get dist-upgrade
       # reboot

Start by cloning the OpenStack-Ansible repository and changing into the
repository root directory:

   .. code-block:: console

       git clone https://github.com/openstack/openstack-ansible \
           /opt/openstack-ansible
       cd /opt/openstack-ansible


Rebuilding an AIO
-----------------
Sometimes it may be useful to destroy all the containers and rebuild the AIO.
While it is preferred that the AIO is entirely destroyed and rebuilt, this
isn't always practical. As such the following may be executed instead:

   .. code-block:: bash

       # Move to the playbooks directory.
       cd /opt/openstack-ansible/playbooks

       # Destroy all of the running containers.
       openstack-ansible lxc-containers-destroy.yml

       # On the host stop all of the services that run locally and not
       #  within a container.
       for i in \
            $(ls /etc/init \
              | grep -e "nova\|swift\|neutron" \
              | awk -F'.' '{print $1}'); do \
         service $i stop; \
       done

