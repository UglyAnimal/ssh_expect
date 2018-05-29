# ssh_expect

The script expect_ssh.sh sets up the ssh connection with all of the hosts from hosts.txt file and executes all the commands from the commands.txt file on each host. At startup it asks to enter username, password and passphrase to establish ssh connection. If the host is unknown then this script adds the fingerprint of this host to the list of known hosts and continues to execute commands.
