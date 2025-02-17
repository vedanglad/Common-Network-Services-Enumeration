# Network File System (NFS)

| Port Numbers | Network Protocol |
| --- | --- |
| `111` | `TCP/UDP` |
| `2049` | `TCP/UDP` |

#### TCP is preferred. UDP is preferred for faster performance.

### Configuration file location
```bash
/etc/exports
```

## Footprinting

### Nmap 

#### TCP Scan
```bash
# Service version (-sV) and default script (-sC) scan
$ sudo nmap -sV -sC -p 111,2049 <TARGET_IP>

# Running all smb nmap Lua scripts using '--script' option
$ sudo nmap -sV --script nfs* <TARGET_IP>
```

#### UDP Scan
```bash
# UDP Scan using '-sU' option
$ sudo nmap -sU -sV -sC -p 111,2049 <TARGET_IP>
```
## Interaction with FTP

### Show Available NFS Shares

```bash
$ showmount -e <TARGET_IP>
```

### Mounting NFS Share

```bash
# Create local directory to mount remote nfs share locally
$ mkdir <path/to/local_nfs_dir>

# Mount command to connect remote nfs share to local directory
$ sudo mount -t nfs <TARGET_IP>:<path/to/remote_nfs> <path/to/local_nfs_dir> -o nolock
```

### Listing NFS contents

```bash
# List Contents with Usernames & Group Names
$ ls -l <path/to/local_nfs_dir>

# List Contents with UIDs & GUIDs
$ ls -n <path/to/local_nfs_dir>
```

### Unmount NFS Share

```bash
$ sudo umount <path/to/local_nfs_dir>
```