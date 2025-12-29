## curl

> **curl** --> A tool for transferring data from or to a server using URLs. It supports 28 protocols.

1. **Websites** --> `curl <website_url>`: Returns the body/source code of the URL.
2. **APIs** --> `curl <api_url>`: Will return the JSON returned from the API.
3. **Local Files** --> `curl <file_url>`: Returns the content of the file.

| Flag | What it does                                       | Example                                                                           |
| ---- | -------------------------------------------------- | --------------------------------------------------------------------------------- |
| `-I` | Fetch only HTTP headers                            | `curl -I https://example.com`                                                     |
| `-i` | Fetch headers and body                             | `curl -i https://example.com`                                                     |
| `-O` | Save the file using the original server filename   | `curl -O https://example.com/index.html`                                          |
| `-o` | Save output to the specified filename              | `curl -o mypage.html https://example.com`                                         |
| `-k` | Ignore SSL certificate errors                      | `curl -k https://self-signed.badssl.com/`                                         |
| `-v` | Verbose (detailed request/response)                | `curl -v https://example.com`                                                     |
| `-L` | Follow redirects like HTTP → HTTPS                 | `curl -L http://example.com`                                                      |
| `-d` | Attach data to be sent and implies a POST request. | `curl -d "param1=value1&param2=value2" https://api.example.com/submit`            |
| `-X` | Specify the HTTP method (PUT, DELETE etc.)         | `curl -X DELETE https://api.example.com/123`                                      |
| `-H` | Headers: Pass tokens, content type, cookies, etc.  | `curl -X POST -H "Content-Type: application/json" https://api.example.com/submit` |
| `-u` | Use basic HTTP auth.                               | `curl -u username:password https://example.com`                                   |

| Flag                | What it does                                            | Example                                                 |
| ------------------- | ------------------------------------------------------- | ------------------------------------------------------- |
| `-F`                | Multipart form upload (like file uploads in HTML forms) | `curl -F "file=@myfile.txt" https://example.com/upload` |
| `-s`                | Silent mode: don’t show progress or errors              | `curl -s https://example.com`                           |
| `-S`                | When used with `-s`, show errors if they happen         | `curl -S https://example.com`                           |
| `-w`                | Format output: show HTTP status code, timing, etc.      | `curl -w "%{http_code}\n" https://example.com`          |
| `-D`                | Save headers to a file                                  | `curl -D headers.txt https://example.com`               |
| `--cert`            | Use a client certificate                                | `curl --cert mycert.pem https://secure.example.com`     |
| `--cacert`          | Use a specific CA bundle                                | `curl --cacert ca.pem https://example.com`              |
| `--max-time`        | Limit the total time allowed for the request            | `curl --max-time 10 https://example.com`                |
| `--connect-timeout` | Limit connection time                                   | `curl --connect-timeout 5 https://example.com`          |
| `--retry`           | Retry up to `<num>` times on transient errors           | `curl --retry 3 https://example.com`                    |
```bash
# Follow redirects, silent mode and format output.
curl -Ls -w "%{http_code}\n" https://example.com

# Silent, download to a file.
curl -s -o output.html https://example.com

# Upload JSON securely with auth.
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer <token>" -d '{"key":"value"}' https://api.example.com/submit
```

## dig
## nslookup
## ip

| Flag    | What it Does              | Example         |
| ------- | ------------------------- | --------------- |
| `-4`    | Show IPv4 only            | `ip -4 a`       |
| `-6`    | Show IPv6 only            | `ip -6 r`       |
| `-s`    | Show statistics           | `ip -s link`    |
| `-d`    | More detailed debug info  | `ip -d addr`    |
| `-br`   | Brief, tabular output     | `ip -br addr`   |
| `-json` | Output in JSON (newer ip) | `ip -json addr` |

| Object         | What it Manages           |
| -------------- | ------------------------- |
| `link`         | Network interfaces        |
| `addr` or `a`  | IP addresses              |
| `route` or `r` | Routing table             |
| `neigh`        | ARP / neighbor cache      |
| `rule`         | Routing policy rules      |
| `maddr`        | Multicast addresses       |
| `mroute`       | Multicast routing         |
| `tunnel`       | Tunnels (GRE, SIT, etc.)  |
| `xfrm`         | IPsec states and policies |
| `monitor`      | Monitor changes live      |
## Common COMMANDS by OBJECT

| Object  | Common Commands                          |
| ------- | ---------------------------------------- |
| link    | `show`, `set up`, `set down`, `set name` |
| addr    | `show`, `add`, `del`                     |
| route   | `show`, `add`, `del`                     |
| neigh   | `show`, `add`, `del`                     |
| monitor | `all`, `link`, `route`, `neigh`          |

## nmap

## netstat
> [!warning] use ss instead

> **netstat** --> displays network connections, routing tables, interface statistics, and more. It’s very useful for checking open ports, active connections, and network diagnostics.

| Command      | What it does                                              |
| ------------ | --------------------------------------------------------- |
| `netstat`    | Show basic active connections (TCP/UDP)                   |
| `netstat -a` | Show all connections + listening ports                    |
| `netstat -t` | Show only TCP connections                                 |
| `netstat -u` | Show only UDP connections                                 |
| `netstat -l` | Show only listening ports                                 |
| `netstat -p` | Show PID/program name for each connection                 |
| `netstat -n` | Show numerical addresses/ports (skip DNS lookup — faster) |
| `netstat -r` | Show routing table                                        |
| `netstat -i` | Show network interface statistics                         |
| `netstat -s` | Show protocol statistics (packets, errors, etc.)          |

```bash
# Show all listening TCP/UDP ports, numerically.
netstat -tuln

# Show all listening TCP/UDP ports with PIDs, numerically.
netstat -tulpn

# Show all active TCP/UDP connections with PIDS, numerically.
netstat -tunap

# Show all listening TCP ports with PIDS, numerically.
netstat -plnt
```

### ss
> **ss** --> A faster, more powerful replacement for netstat (which is deprecated). It supports most of the same flags, plus advanced built-in filtering.

1. **Built-in Filtering**: It has filters like `state`, `sport`, `dport`, `src` and `dst` so no need to use `grep`.
2. **Numerical sorting by Default**: No need to use `-n` flag.
3. Use `ip route`/`ip -s link` command instead of `netstat -r`/`netstat -i`.

| Filter                     | Usage                               |
| -------------------------- | ----------------------------------- |
| `state <STATE>`            | By state (ESTABLISHED, LISTEN etc.) |
| `sport = :<port>`          | By source port                      |
| `dport = :<port>`          | By destination port                 |
| `src <ip>`                 | By source ip                        |
| `dst <ip>`                 | By destination ip                   |
| `<filter> '( <filter> )'`  | Nesting filters                     |
| `( <filter> or <filter> )` | Logical OR                          |
```bash
# Show incoming connections from port 22 (SSH)
ss -t state ESTABLISHED '( sport = :22 )'

# Show outgoing connections to port 22 (SSH)
ss -t state ESTABLISHED '( dport = :22 )'

# Show incoming AND outgoing connections from port 22 (SSH)
ss -t state ESTABLISHED '( sport = :22 or dport = :22)'

# Show all listening TCP sockets
ss -t state LISTEN
```
