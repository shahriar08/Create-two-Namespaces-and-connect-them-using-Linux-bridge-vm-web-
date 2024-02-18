# Create-two-Namespaces-and-connect-them-using-Linux-bridge-vm-web-

Create two namespaces
```
ip netns add ns1
ip netns add ns2
```

Create a Linux bridge
```
ip link add br0 type bridge
```

Move one end of the veth pair to ns1 and the other end to the bridge
```
ip link add veth0 type veth peer name veth1
ip link set veth0 netns ns1
ip link set veth1 master br0
```
Move one end of another veth pair to ns2 and the other end to the bridge
```
ip link add veth2 type veth peer name veth3
ip link set veth2 netns ns2
ip link set veth3 master br0
```
Configure IP addresses inside the namespaces
```
ip netns exec ns1 ip addr add 10.0.0.1/24 dev veth0
ip netns exec ns2 ip addr add 10.0.0.2/24 dev veth2
```
Bring up the interfaces inside the namespaces
```
ip netns exec ns1 ip link set veth0 up
ip netns exec ns2 ip link set veth2 up
```
Bring up the bridge interface
```
ip link set br0 up
```
