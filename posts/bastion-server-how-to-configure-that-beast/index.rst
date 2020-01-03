.. title: Bastion server - how to configure that beast
.. slug: bastion-server-how-to-configure-that-beast
.. date: 2017-04-03 09:00:00 UTC-05:00
.. tags: osic, networking
.. category: tutorials
.. link: 
.. description: Configuring and using jumpbox/bastion to access private servers
.. type: text

I was thinking about this post for a long time, especially that it's my first
one on this blog. To not make it too long and too complicated I'm gonna just
jump into the topic.

Bastion, sometimes also named as jumpbox, is one of the very important
things in internet and clouds. When we have the edge router in a home and we
would like to connect to the server somewhere there what we gonna do? We need
to use bastion - in this specific situation, it's gonna be our router. In other
situation, it's gonna be a server which is connected to the public network or
just server which is allowed to access thru the firewall.

.. TEASER_END

How to properly configure your computer and this server to make it usable?
It's gonna require from us just one configuration - your ssh config file located
usually in *~/.ssh/config*. In this file, you can specify a lot of options.
To access complete documentation you can read *man ssh_config* or here_.

Let's do it. What we need to configure in ssh config:

.. code-block:: bash

    Host                    bastion
    ForwardAgent            yes

    Host                    host-behind-firewall
    ProxyCommand            ssh -W %h:%p -q bastion

We're using here two specific configuration options:

* ForwardAgent
* ProxyCommand

These options enable whole magic for us. *ForwardAgent* like the name says
enables forwarding of your local ssh-agent into the remote server. It means
that your locally loaded ssh keys, ssh config are sent to the remote host to
allow for the reusability there. The *ProxyCommand* allows us to execute some
commands before real sshing into the remote machine. 
How we're gonna use this:

.. code-block:: bash

   ssh host-behind-firewall

The agent will execute *ProxyCommand*. At first, it's gonna ssh us to
*bastion* server. To connect to this server agent will read our config file and
execute all specified options - it's gonna *forward agent* as specified in
*bastion* configuration section. Because of this, we will be able to use our
keys to connect to next server. Next *ProxyCommand* option *-W* will
automatically after establishing a connection to *bastion* connect us to our
destination server *host-behind-firewall*.

As our local ssh-agent is forwarded to *bastion* we can use the same ssh key to
login to both of servers, as also we can in *config* file specify some
different key to be used to connect. Biggest win of this configuration is that
we just need to write 4 lines of configuration and never again copy any keys
around the servers, especially if there is a lot of them and many people has
access to them with same privileges.

.. _here: https://www.freebsd.org/cgi/man.cgi?query=ssh_config&sektion=5
