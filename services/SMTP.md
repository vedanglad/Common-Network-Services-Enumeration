# Simple Mail Transfer Protocol (SMTP)

| Protocol | Ports |
| --- | --- |
| Standard | `25/TCP` |
| SMTP with Implicit TLS (encrypted from first) | `465/TCP` |
| SMTPS (ability to switch to encrypted channel using `STARTTLS`) | `587/TCP` |
| Alternative if above ports blocked. Not officially associated. | `2525/TCP` |

### Configuration file location
```bash
/etc/postfix/main.cf
```

#### Truncating all comments and empty lines from the configuration file and displaying the content
```bash
$ cat /etc/postfix/main.cf | grep -v "#" | sed -r "/^\s*$/d"
```

## Footprinting SMTP

### Nmap

```bash
$ sudo nmap -sV -sC -p 25 <TARGET_IP>
```

### Nmap - Open Relay Nmap Script

```bash
$ sudo nmap -sV -p 25 --script smtp-open-relay <TARGET_IP>
```

## Interaction with SMTP

### Telnet
```bash
$ telnet <TARGET_IP> 25
```

### OpenSSL
To connect using the TLS protocol on port `587`:
```bash
$ openssl s_client -starttls smtp -connect <TARGET_IP>:587
```

To use SSL on port `465`:
```bash
$ openssl s_client -connect <TARGET_IP>:465
```