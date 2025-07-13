# Network


## Network addresses


`ip`  manages system network connections (replaces the ifconfig command), `ip [options] object command`.

Show the IP addresses of all the network connections available, including container systems e.g. docker.

```shell
ip -brief address
```

```shell-output
❯ ip -brief address
lo               UNKNOWN        127.0.0.1/8 ::1/128
enp2s0f0         DOWN
enp5s0           UP             192.168.0.238/24 fe80::3af3:abff:fef3:4f5e/64
wlp3s0           DOWN
docker0          UP             172.17.0.1/16 fe80::4dc:7aff:fe24:47a0/64
veth5a2b981@if2  UP             fe80::3c4e:ebff:fee4:56bf/64
enxd0c1b51d66d5  DOWN
```

> `ip address` show details of each network connection.

`ip route` to show routing table entries.

```shell-output
❯ ip route
default via 192.168.0.1 dev enp5s0 proto dhcp src 192.168.0.238 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1
192.168.0.0/24 dev enp5s0 proto kernel scope link src 192.168.0.238 metric 100
```


## Testing connections

 ping command

The ping command sends packets to a target server and fetches the responses. It is helpful for network diagnostics. The basic syntax looks like the following:

ping [option] [hostname_or_IP_address]

By default, ping sends infinite packets until the user manually stops it by pressing Ctrl + C.

However, you can specify a custom number using the -c option. You can also change the interval between transfers by adding -i.

For instance, let’s send 15 packets every two seconds to Google’s server:

ping -c 15 -i 2 google.com

ping command sends packets to google with custom settings




## Remote Data Access

`wget` to download files from the internet via HTTP, HTTPS, or FTP protocols, `wget [options] [URL]`



`curl` to transfer data from or to a server by specifying its URL, `curl [options] URL`

-O or -o to download files from the specified link.

> Running cURL without an option will print the website’s HTML content in your Terminal.



### Testing APIs

`–X` followed by an HTTP method (GET, POST, etc.) to test API endpoints.


Request data from a specified API endpoint

```shell
curl -X GET https://api.example.com/endpoint
```

`-d` to Post data to an API endpoint

```shell

```

<!-- TODO: rewrite examples

Use -d to set data, -X to mention method

Send form data (default) application/x-www-form-urlencoded with POST

curl -d 'name=Sugar&category_id=3' http://example.com/api/products
curl -X PUT -d 'name=Sugar&category_id=3' http://example.com/api/products

Field values can be mentioned separately.

curl -d name=Sugar -d category_id=3 http://example.com/api/products

Submit as JSON Request Body

curl -d '{"name":"Sugar", "category_id":3}' -H 'Content-Type: application/json' http://example.com/api/products
curl -X PUT -d '{"name":"Sugar", "category_id":3}' -H 'Content-Type: application/json' http://example.com/api/products
 -->

[Curl tutorial](https://curl.se/docs/tutorial.html){target=_blank .md-button}


## Secure file copy

scp to securely copy files and directories between systems over a network.

```shell
scp [option] [source username@IP]:/[directory and file name] [destination username@IP]:/[destination directory]
```

If you are copying items to or from your local machine, omit the IP and path. When transferring a file or folder from a local machine, specify its name after options.

For example, we will run the following to copy file1.txt to our VPS’ path/to/folder directory as root:

scp file1.txt root@185.185.185.185:path/to/folder

You can change the default SCP port by specifying its number after the -P option. Meanwhile, use the -l flag to limit the transfer bandwidth and add –C to enable compression.
48. rsync command

The rsync command syncs files or folders between two destinations to ensure they have the same content. The syntax looks as follows:

rsync [options] source destination

The source and destination can be a folder within the same system, a local machine, or a remote server. If you are syncing content with a VPS, specify the username and IP address like so:

rsync /path/to/local/folder/ vps-user@185.185.185.185:/path/to/remote/folder/

You can add the -a option to sync the file or folder’s attributes as well, including their symbolic links. Meanwhile, use the -z flag to enable compression during the transfer.
50. netstat command

The netstat command displays information about your system’s network configuration. The syntax is simple:

netstat [options]

Add an option to query specific network information. Here are several flags to use:

    -a – displays listening and closed sockets.
    -t – shows TCP connections.
    -u – lists UDP connections.
    -r – displays routing tables.
    -i – shows information about network interfaces.
    -c – continuously outputs network information for real-time monitoring.

51. traceroute command

The traceroute command tracks a packet’s path when traveling between hosts, providing information like the transfer time and involved routers. Here’s the syntax:

traceroute [options] destination

You can use a hostname, domain name, or IP address as the destination. If you don’t specify an option, traceroute will run the test using the default settings.

Change the maximum packet hops using the -m option. To prevent traceroute from resolving IP addresses, add -n.

You can also enable a timeout in seconds using the -w flag followed by the duration.
52. nslookup command

The nslookup command requests a domain name system (DNS) server to check a domain linked to an IP address or vice versa. Here’s the syntax:

nslookup [options] domain-or-ip [dns-server]

If you don’t specify a DNS server, nslookup will use your internet service provider’s default resolver. You can add other options to change how this command queries an IP address or a domain.

For example, use the -type= option to specify the information you want to check, such as the DNS records.

You can also set up automatic retry with the -retry= flag and add -port= to use a specific port.
nslookup command resolves a domain name to an IP address

Since some Linux distros don’t have this utility pre-installed, you might encounter the “command not found” error. You can configure it by downloading bind-utils or dnsutils via your package manager.
53. dig command

The domain information groper or dig command displays information about a domain. It is similar to nslookup but more comprehensive. The syntax looks as follows:

dig [options] [server] [type] name-or-ip

Running dig without an argument will check A records of the specified domain using the operating system’s default resolver. You can query a particular record by specifying it in the [type] argument like the following example:

dig MX domain.com

To run a reverse DNS lookup, add the –x option and use an IP address as the target.
