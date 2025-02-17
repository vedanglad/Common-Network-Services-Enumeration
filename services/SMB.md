# Server Message Block (SMB)

| Implementation | Port Number |
| --- | --- |
| **Older** | `139/TCP` |
| **Current** | `445/TCP` |

### Configuration file location
```bash
/etc/samba/smb.conf
```

#### Truncating all comments and empty lines from the configuration file and displaying the content
```bash
$ cat /etc/samba/smb.conf | grep -v "#\|\;"
```

## Footprinting

### Nmap 

```bash
# Service version (-sV) and default script (-sC) scan
$ sudo nmap -sV -sC -p 139,445 <TARGET_IP>

# Running all smb nmap Lua scripts using '--script' option
$ sudo nmap -sV --script smb* <TARGET_IP>
```

### RPCclient

#### Rpcclient - Server/Domain/Share Enumeration

```bash
# Logging in RPC without credentials
$ rpcclient -U "" -N <TARGET_IP>

# Get server information
rpcclient $> srvinfo

# Get information of all the domains deployed in the network
rpcclient $> enumdomains

# Get domain, server, and user information of all the deployed domains
rpcclient $> querydominfo

# Get information about all available shares
rpcclient $> netshareenumall

# Get information about a specific share
rpcclient $> netsharegetinfo <share_name>
```

#### Rpcclient - User Enumeration

```bash
# Get information about all domain users
# Outputs usernames and RIDs
rpcclient$> enumdomusers

# Get information about a specific user using RID
rpcclient$> queryuser <USER_RID>
```

#### Rpcclient - Group Information
The group rid obtained from the user enumeration can be used to obtain group information.
```bash
# Get information about a specific group using RID
rpcclient$> querygroup <GROUP_RID>
```

#### Brute Forcing User RIDs using `BASH` oneliner
```bash
$ for i in $(seq 500 1100); do rpcclient -N -U "" <TARGET_IP> -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo ""; done
```

### Impacket - Samrdump.py

```bash
### Impacket is available in Kali Linux by default
$ impacket-samrdump <TARGET_IP>

### Or ###

# Download using wget
$ wget https://raw.githubusercontent.com/fortra/impacket/refs/heads/master/examples/samrdump.py -qO samrdump.py

# Make it executable
$ chmod +x samrdump.py

# Run the script
$ ./samrdump.py <TARGET_IP>

```

### SMBclient
```bash
# Listing shares without credentials
$ smbclient -N -L //<TARGET_IP>
```

### SMBmap

```bash
$ smbmap -H <TARGET_IP>
```

### CrackMapExec

```bash
$ crackmapexec smb <TARGET_IP> --shares -u '' -p ''
```

### Enum4Linux-ng - Enumeration

```bash
### Install from Git
$ git clone https://github.com/cddmp/enum4linux-ng.git
$ cd enum4linux-ng
```

```bash
### Recommended to create a python virutual environment before running pip3 for installing dependencies

# Create virtual environment "enum4linux_venv"
$ python3 -m venv enum4linux_venv

# Active virtual environment
$ source enum4linux_venv/bin/activate

# Deactivate virtual environment
$ deactivate
```

```bash
### Verify the virtual environment is activated by looking at the command prompt 
### that should have the virtual environment name inside brackets.

# Install dependencies
(enum4linux_venv) kali@kali: $ pip3 install -r requirements.txt
```

### Execute enum4linux-ng

```bash
(enum4linux_venv) kali@kali: $ ./enum4linux-ng.py <TARGET_IP_> -A
```

## Interaction with SMB

### SMBclient

#### Connecting to the SMB Share
```bash
# Without credentials
$ smbclient -N //<TARGET_IP>/<SHARE_NAME>

# With credentials
$ smbclient //<TARGET_IP>/<SHARE_NAME> -U <USERNAME>
```
Adjust the '/' in the path according to the OS. Increase or decrease the number of '/' as required.

#### SMB Commands
```bash
# Listing files
smb: \> ls

# Changing directory
smb: \> cd <DIRECTORY_NAME>

# Downloading files
smb: \> get <FILE_NAME>

# Uploading files
smb: \> put <FILE_NAME>
```
