#!/usr/bin/expect -f

set timeout 10
send_user "Enter Username: "
expect_user -re "(.*)\n"
set USER $expect_out(1,string)

send_user "Enter user password: "
stty -echo
expect_user -re "(.*)\n" {set PASS $expect_out(1,string)}
send_user "\n"
stty echo

send_user "Enter SSH passphrase: "
stty -echo
expect_user -re "(.*)\n" {set PASSPHRASE $expect_out(1,string)}
send_user "\n"
stty echo

#Get the list of hosts one per line
set f [open "hosts.txt"]
set hosts [split [read -nonewline $f] "\n"]
close $f

#Get the commands to run one per line
set f [open "commands.txt"]
set commands [split [ read $f] "\n"]
close $f

foreach host $hosts {

spawn ssh $USER@$host
  expect {
          "(yes/no)?*" { send "yes\r" }
          "Enter passphrase for key*" { send "$PASSPHRASE\r" }
          "Could not resolve hostname*" {send_user "Resource temporarily unavailable\n"; continue }
          "ssh: connect to host $host port 22: Resource temporarily unavailable" {send_user "Resource temporarily unavailable\n"; continue}
          "ssh: connect to host $host port 22: No route to host" {send_user "No route to host\n"; continue}
          "assword:" {send "$PASS\r"; expect "Permission denied, please try again." {send_user "Wrong password\n"; continue}}
         }

 foreach cmd $commands {
        expect "#"
        send "$cmd\r"
}


  expect "#"
  send "exit\r"
  expect eof
}
