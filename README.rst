###########################################################################
openssl-gensconf - A program to create OpenVPN configuration files and keys
###########################################################################

I looked around and I found several programs, but none were really very
suitable.  So I wrote my own.  It's pretty simple, but it does the job.

You will need to get easyrsa from::
   https://github.com/OpenVPN/easy-rsa
and put it into your path.  This is not the same thing as the easy-rsa package
on Ubuntu or other distros.  The easiest way to do this is to check out the git
repository and make a link from easyrsa3/easyrsa there to your bin directory.

This program creates keys and configuration files for use with OpenVPN.  To use
this, create a directory and cd into it.  Then run the program with the form::
    openvpn-genconfg [<client1> [<client2> ...]]
On the first run it will ask for configuration and then generate server keys
and configuration.  Then for each parameter supplied it will generate a client
.ovpn file.

Only RSA is supported for now.  It would be easy to add the other key types
that easyrsa supports, but I'm not using it, so I didn't see a big need.

The program will generate a tarball with the server config.  It will tell you
the filename.  Untar that in /etc/openvpn on your target.

It will also create a .opvn file for each client with all the keys and
configuration in it.  OpenVPN knows how to use these files, transfer them
(securely) to the clients and load them into OpenVPN.

NOTE: Do not run this on the OpenVPN server.  The CA key is critical to keep a
secret.  Do it on a secure internal machine.

If you need to revoke certificates, you can use easyrsa to create a crl.  Use
the command::
  easyrsa revoke <client>
then run::
  easyrsa gen-crl
Copy the crl.pem file created by easyrsa (it will tell you the name) onto the
server and replace the file /etc/openvpn/<serverfile>-keys/crl.pem on the
target.
