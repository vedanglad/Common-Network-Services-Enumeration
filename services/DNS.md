# Domain Name System (DNS)

| Port Numbers | Network Protocol |
| --- | --- |
| `53` | `UDP` |

#### Uses `UDP` first. If can’t communicate uses `TCP`.

### Configuration file location
```bash
/etc/hosts
```

### Bind9 DNS Server Configuration file location
```bash
/etc/bind/named.conf.local
```

## Footprinting DNS

### Dig Command

#### NS records.
Get nameserver records using `ns` option.
`@<DNS_SERVER>` to specify which DNS server to query to.

```bash
dig ns <TARGET_IP/DOMAIN> @<DNS_SERVER>
```

#### Version Query
Get DNS server's version using a class `ch` query and type `txt`.

```bash
dig ch txt version.bind <TARGET_IP/DOMAIN>
```

#### ANY Query
Use option `any` to view all available records.

```bash
# Command
dig any <TARGET_IP/DOMAIN> @<DNS_SERVER>
```

#### AXFR Zone Transfer
Perform a DNS zone transfer using `axfr` option.

```bash
# Command
dig axfr <TARGET_IP/DOMAIN> @<DNS_SERVER>
```

#### Subdomain Brute Forcing using Bash command

```bash
for sub in $(cat <WORDLIST_PATH>); do dig $sub.<DOMAIN> @<DNS_SERVER> | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a <OUTPUT_FILE_PATH>; done
```

### DNSenum

```bash
dnsenum --dnsserver <TARGET_IP/DOMAIN> --enum -p 0 -s 0 -o <OUTPUT_FILE_PATH> -f <WORDLIST_PATH> <DOMAIN>
```