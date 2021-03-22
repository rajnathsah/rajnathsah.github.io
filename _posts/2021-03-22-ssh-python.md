---
title: Command execution over SSH
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
## Solution/workaround
