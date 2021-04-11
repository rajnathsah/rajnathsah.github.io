---
title: Command execution over SSH using Python libraries.
date: 2021-03-22 00:00:00 +0000
description: Execute command and script over SSH using python library.
img:  # Add image post (optional)
classes: wide
tags: [SSH, Python, Paramiko, Asyncssh] # add tag
---
# SSH
SSH or Secure Shell is a cryptographic network protocol for operating network services securely over an unsecured network.[1] Typical applications include remote command-line, login, and remote command execution, but any network service can be secured with SSH.  
# Remote execution
Python has many libraries for using SSH. I will discuss about the two libraries which i have used in my projects.  
## Paramiko
Paramiko is a Python (2.7, 3.4+) implementation of the SSHv2 protocol [1], providing both client and server functionality. While it leverages a Python C extension for low level cryptography (Cryptography), Paramiko itself is a pure Python interface around SSH networking concepts.  
Let us look at example of paramiko.  
```python
import paramiko

hostname = 'localhost'
username = 'root'
password = 'password'
script_path = "/root"
command = "ls"
pem = None

# ssh client
client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

# connect to ssh server
client.connect(hostname =hostname,
                username=username,
                password=password,
                pkey=paramiko.RSAKey.from_private_key_file(pem) if pem else None)

# run command
_, stdout, stderr = client.exec_command('cd {} && {}'.format(script_path, command))

exit_code = stdout.channel.recv_exit_status()
out = stdout.readlines()
err = stderr.readlines()
client.close()
```
## Asyncssh
AsyncSSH is a Python package which provides an asynchronous client and server implementation of the SSHv2 protocol on top of the Python 3.6+ asyncio framework. Lets look into one simple example to run command using asynssh.  
```python
import asyncio, asyncssh, sys

hosts = 'localhost'
username = 'username'
password = 'password'

# create connection
async def run_client():
    conn = await asyncio.wait_for(asyncssh.connect(hosts, username=username, password=password, known_hosts = None),10,)
    return conn

# run command
async def run_command():    
    try:
        conn = await run_client()        
        result = await conn.run('ls')

        if result.exit_status == 0:            
            print(result.stdout, end='')
        else:
            print(result.stderr, end='', file=sys.stderr)
            print('Program exited with status %d' % result.exit_status,
                file=sys.stderr)

    except Exception as ex:
        print(ex)      

async def program():
    await asyncio.gather(run_command())

# Run our program until it is complete
try:
    asyncio.get_event_loop().run_until_complete(program())
except (OSError, asyncssh.Error) as exc:
    sys.exit('SSH connection failed: ' + str(exc))
```
## Issue with long running commands
Above examples works well in most cases and it worked for me also, but in coporate network where everything is behind firewall, these firewalls causes issue in case of long running commands/scripts. Here firewall means the firewall software which are deployed on corporate networks. These firewall monitors network connections and if there is a long running idle sessions, then it disconnects automatically. It is a security measure but it can cause problems for programs which takes longer and there is communication between client and server. It looks simple but debugging this issue is bit tidious. For me it tooks weeks to come to this conclusion and then i started looking for solution. SSh has option to keep pinging after certain time but unfortunately, i could not find it anywhere.
## Solution/workaround
Solution to this problem is very simple, We need to keep session alive by doing something during the long running idle session. Either you return something or pass something but bit tedius to implement. But ssh has very nice option to send ping after given interval, this helps in keeping the session active and alive. We see an example using paramiko where using transport, keep alive interval can be set.  
* Example using paramiko  
```python
import paramiko

# Remote server details
hostname = '172.*.*.*'
port = 22
username = 'root' 
password = 'password'
# script and path
script_path = "/root/raj/"
command = "sh sleeptest.sh"

# set 60 sec interval to ping remote
client = paramiko.Transport((hostname, port))
client.set_keepalive(60)
client.connect(username=username, password=password)

stdout_data = []
stderr_data = []
session = client.open_channel(kind='session')
session.exec_command('cd {} && {}'.format(script_path, command))

stdout = session.makefile('rb', -1)
stderr = session.makefile_stderr('rb', -1)

print('exit status: ', session.recv_exit_status())

out = stdout.readlines()
err = stderr.readlines()

session.close()
client.close()
```
*  Example using asyncssh
```python
import asyncio, asyncssh, sys

async def run_client():
    conn = await asyncio.wait_for(asyncssh.connect('172.*.*.*', username='root', password='password', known_hosts = None,
                                                    keepalive_interval=600, keepalive_count_max=10000),10,)

    return conn

async def run_command():    
    try:
        conn = await run_client()        
        result = await conn.run('cd /root/raj && sh sleeptest.sh')

        if result.exit_status == 0:            
            print(result.stdout, end='')                        
        else:
            print(result.stderr, end='', file=sys.stderr)
            print('Program exited with status %d' % result.exit_status,
                file=sys.stderr)
    except Exception as ex:
        print(ex)      

async def program():
    # Run both print method and wait for them to complete (passing in asyncState)    
    await asyncio.gather(run_command())

# Run our program until it is complete
loop = asyncio.get_event_loop()
loop.run_until_complete(program())
loop.close()
```
Hope it will be helpful to anyone new to these library. Do let me know if you find this article useful and feel free to reach me using any medium to discuss on anything.  
Happy coding!
