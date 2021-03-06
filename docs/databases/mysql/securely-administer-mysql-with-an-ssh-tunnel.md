---
author:
  name: Linode
  email: docs@linode.com
description: 'Accessing MySQL databases remotely using an SSH tunnel.'
keywords: 'MySQL tunnel,MySQL over SSH,SSH tunnel'
license: '[CC BY-ND 3.0](http://creativecommons.org/licenses/by-nd/3.0/us/)'
alias: ['databases/mysql/mysql-ssh-tunnel/']
modified: Thursday, August 22nd, 2013
modified_by:
  name: Linode
published: 'Wednesday, January 6th, 2010'
title: Securely Administer MySQL with an SSH Tunnel
---

This guide will show you how to make a secure connection to your remote MySQL server from your local computer, using an *SSH tunnel*. This is useful if you want to use administration tools on your local computer to do work on your server.

After following these instructions, you'll be able to connect to `localhost` on your workstation using your favorite MySQL management tool. The connection will be securely forwarded to your Linode over the Internet.

Prerequisites
-------------

-   [MySQL](/docs/hosting-website#sph_installing-mysql) is installed.
-   MySQL is configured to listen on `localhost` (127.0.0.1). This is enabled by default.

Create a Tunnel with PuTTY on Windows
-------------------------------------

This section will show you how to create an SSH tunnel to MySQL on Windows, using the PuTTY tool.

### Setting Up the Tunnel

First, you need to establish a basic connection to your Linode:

1.  Download [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
2.  Save PuTTY to your desktop.
3.  Double-click the PuTTY file to begin - no need to install. You will see the following window:

    [![The session login screen in PuTTY on Windows.](/docs/assets/361-putty-01-session.png)](/docs/assets/361-putty-01-session.png)

4.  Enter the hostname or IP address of your Linode in the **Host Name (or IP address)** field.
5.  In the left-hand menu, go to **Connection -\> SSH -\> Tunnels**.
6.  In the **Source port** field, enter `3306`.
7.  In the **Destination** field, enter `127.0.0.1:3306`. See the final configuration below:

    [![Tunneling a remote MySQL connection through SSH with PuTTY on Windows.](/docs/assets/363-putty-04-mysql-ssh-tunnel.png)](/docs/assets/363-putty-04-mysql-ssh-tunnel.png)

8.  Click **Open** to start the SSH session.
9.  If you haven't logged in to this system with PuTTY before, you will receive a warning similar to the following. Verify that this server is the one to which you want to connect, then click **Yes**:

    [![An unknown host key warning in PuTTY on Windows.](/docs/assets/362-putty-02-host-key-warning.png)](/docs/assets/362-putty-02-host-key-warning.png)

    {:.note}
    >
    > This warning appears because PuTTY wants you to verify that the server you're logging in to is who it says it is. It is unlikely, but possible, that someone could be eavesdropping on your connection and posing as your Linode. To verify the server, compare the key fingerprint shown in the PuTTY warning - the string of numbers and letters starting with **ssh-rsa** in the image above - with your Linode's public key fingerprint. To get your Linode's fingerprint, log in to your Linode via the AJAX console (see the **Console** tab in the Linode Manager) and executing the following command:
    >
    > ssh-keygen -l -f /etc/ssh/ssh\_host\_rsa\_key.pub

    > The key fingerprints should match. Once you click **Yes**, you won't receive further warnings unless the key presented to PuTTY changes for some reason; typically, this should only happen if you reinstall the remote server's operating system. If you receive this warning again for the same Linode after the key has already been cached, you should not trust the connection and investigate matters further.

10. Direct your local MySQL client to `localhost:3306`. Your connection to the remote MySQL server will be encrypted through SSH, allowing you to access your databases without running MySQL on a public IP.

Create a Tunnel with mysql-tunnel on Mac OS X or Linux
------------------------------------------------------

This section will show you how to create an SSH tunnel to MySQL on Mac OS X or Linux, using the mysql-tunnel tool.

1.  Open a command prompt and run the following command to open the SSH tunnel:

        ssh -L 127.0.0.1:3306:127.0.0.1:3306 user@example.com -N

    Replace <**user@example.com*>\* with your SSH username and your server's hostname or IP address. The long string of numbers in the command lists the local IP, the local port, the remote IP, and the remote port, separated by colons (**:**).

    {:.note}
    >
    > If you're already running a local copy of MySQL on your workstation, use a different local port (3307 is a common choice). Your new command would look like this:
    >
    >     ssh -L 127.0.0.1:3307:127.0.0.1:3306 user@example.com -N

2.  Direct your local MySQL client to `localhost:3306`. Your connection to the remote MySQL server will be encrypted through SSH, allowing you to access your databases without running MySQL on a public IP.
3.  When you're ready to close the connection, issue a **CTRL-C** command or close the command prompt window. This will close the SSH tunnel.

### Persistent SSH Connections

If you need a persistent SSH tunnel, consider using [autossh](http://www.harding.motd.ca/autossh/). autossh starts and monitors an SSH connection, and restarts it if necessary.

More Information
----------------

You may wish to consult the following resources for additional information on this topic. While these are provided in the hope that they will be useful, please note that we cannot vouch for the accuracy or timeliness of externally hosted materials.

- [Using PuTTY](/docs/networking/using-putty#using_ssh_tunnels)
- [MySQL Documentation](http://dev.mysql.com/doc/)
- [autossh](http://www.harding.motd.ca/autossh/)



