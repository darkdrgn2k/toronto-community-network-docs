# Babel

[Babel](https://www.irif.fr/~jch/software/babel/) is a loop-avoiding distance-vector routing protocol. It does link cost estimation and redistribution of routes from other routing protocols. 

The network uses [jech](https://github.com/jech/babeld) implementation of Bable service alled Babeld. Packaged are compiled from source and packaged. Deb packages pused to [experimental repository]( http://node2.e-mesh.net/deb/repos/apt/debian/pool/main/b/babeld/) after being built using scripts in [package](https://github.com/darkdrgn2k/packages/tree/master/babeld) repo.

Package for EdgeRouter X with ui can be found at https://github.com/darkdrgn2k/RouterX-Babeld-Package.

Prototype babled configuration can be generated at http://node2.e-mesh.net/CONF/ for both openwrt and linux.

## Usage

Babled is only when

- You are NOT using the IP address provided by the Supernode
- You are NOT routing any other IP subnets (neighbourhood node)
- You are NOT behind a NAT
- You want to announce another ip subnet

## Diagnostics

Depending what port the service started on (`local-port` or `-G` options) you can access babeld's console using on of the following. (assuming 999 is the port)

- `nc :: 999`
- `telnet :: 999`

**NOTE** some distributions `nc` does not support IPv6 and it will not work

The command `dump` in the console will list all the current configurations of BABELD

### Explenation of dump

`add interface <INT> up true ipv6 <IPv6> ipv4 <IPv4>`  
This indicates that the interfaces `<INT>` will be used to find other 
Babled nodes. `<IPv6>` and `<IPv4>` are required for routing traffic.  If one is missing check your interface configuration.

`add interface <INT> up false`
This means the interface is added but is not functional (cable not plugged in, or simply down)

`add neighbour f3ecb0 address <IPv6> if <INT> reach ffff ureach 0000 rxcost 96 txcost 96 cost`  
Ideitifies the nodes it found directly connected to it. `<IPv6>` is the local link ip found on the remote nodeand `<INT>` is the interface this link was found on.

`add xroute...metric 256`  
Defines the routes your node is announcing. `metric 256` is the cost that it is announced as

`add route ...`  
This is details about the routes that babeld knows about. `installed yes` or `installed no` defines if this route is being used and installed in the current route table. Make note of `metric` numbres as they inform if the link will be used or not.
