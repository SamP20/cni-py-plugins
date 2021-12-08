# cni-py-plugins
CNI plugins written in python

# Installation

```
mkdir /usr/local/lib/cni/
cp cni-nftables-* /usr/local/lib/cni/
```

```
{
  "cniVersion": "0.4.0",
  "name": "container_net",
  "plugins": [
    {
        "type": "ptp",
        "ipMasq": false,
        "ipam": {
            "type": "host-local",
            "subnet": "172.16.1.0/24",
            "routes": [
                { "dst": "0.0.0.0/0" }
            ]
        }
    },
    {
      "type": "cni-nftables-portmap",
      "capabilities": {"portMappings": true},
      "nftablesConfig": {
        "table": "router",
        "mapname": "port_forwards"
      }
    }
  ]
}
```

Note that `ipMasq` must be set to false as this uses IPTables rules.

The portmap plugin uses a firewall mapping to map source ports to destinations. The map is defined like so:

```
map port_forwards {
    type inet_service: ipv4_addr . inet_service;
}
```