# File Transfer Protocol (FTP)
| Channel | Port Number |
| --- | --- |
| **Command** | `21/TCP` |
| **Data channel (Default)** | `20/TCP` |

Configuration file location

```bash
/etc/vsftpd.conf
```

Not-allowed users file - Usernames mentioned in this file are not allowed to login to FTP.

```bash
/etc/ftpsuers
```

## Footprinting

### Nmap

```bash
# Service version (-sV) and default script (-sC) scan
$ sudo nmap -sV -p21 -sC -A <TARGET_IP_IP>

# Running all ftp nmap Lua scripts using '--script' option
$ sudo nmap -p21 -sV --script ftp* <TARGET_IP>
```

### Interaction with FTP

### ftp

```bash
# Connecting locally.
$ ftp <TARGET_IP>

# Connecting to remote machine.
$ ftp <USER>@<TARGET_IP>

# Connecting to remote machine using anonymous login, if anonymous login enabled.
# Just press enter when prompted for password.
$ ftp anonymous@<TARGET_IP>
```

### telnet

```bash
telent <TARGET_IP> 21
```

### netcat

```bash
nc -nv <TARGET_IP> 21
```

### openssl

```bash
openssl s_client -connect <TARGET_IP> -starttls ftp
```

### Other useful ftp enumeration and interaction command

### `ftp` commands after logging in.

```bash
# Listing files
ls

# Listing recursively
ls -R

# Get ftp server settings overview
status

# Download file from ftp server
get <FILE_NAME>

# Upload file to the ftp server
put <FILE_NAME>

# View file contents without downloading it.
more <FILE_NAME>
```

### Download all available files from the FTP server using `wget`

```bash
$ wget -m --no-passive ftp://<USER>:<PASSWORD>@<TARGET_IP_IP>
```