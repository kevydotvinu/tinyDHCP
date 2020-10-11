# tinyDHCP
A containerized DHCP server using Alpine and Dnsmasq.

## Prerequisites packages
```bash
* buildah
* podman
```

## Configure container storage (Optional)
```bash
su - root
vgcreate vgDATA /dev/vdb
lvcreate -n lvLIB -L 5G /dev/vdb
lvcreate -n lvRUN -L 5G /dev/vdb
mkfs -t xfs -n ftype=1 /dev/vgDATA/lvLIB
mkfs -t xfs -n ftype=1 /dev/vgDATA/lvRUN
mount /dev/vgDATA/lvLIB /var/lib/containers
mount /dev/vgDATA/lvRUN /var/run/containers
```

## Usage
### Open firewall port
```bash
firewall-cmd --add-service=dhcp --permanent
firewall-cmd --reload
```

### Clone git repository
```bash
git clone https:github.com/kevydotvinu/tinyDHCP
cd tinyDHCP
```

### Build container image
```bash
buildah bud --security-opt label=disable --tag localhost/kevydotvinu/tinydhcp:v1 .
```

### Run container image
```bash
podman run --rm \
           --interactive \
           --tty \
           --privileged \
           --net host \
           --volume "$(pwd)/dnsmasq.conf.dhcpproxy:/etc/dnsmasq.conf" \
           --security-opt label=disable \
           --name tinydhcp localhost/kevydotvinu/tinydhcp:v1
```
