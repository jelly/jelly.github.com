---
layout: post
title: "Linux KVM VDE2 setup"
date: 2013-11-30 16:45
comments: false
categories: [archlinux kvm vde2 virtualization]
---

Since I started working at my internship, I needed a way to fully access my virtual machines via a network. I tried to setup a bridge with NetworkManager, but this failed miserably and I couldn't find any documentation online on how to achieve this.

After some browsing I soon found out I could use VDE2 to create a "private" virtual network, where I can access my virtual machines directly via an internal ip. And uses dnsmasq for dhcp for the VM's since I'm too lazy to setup static ip addresses.
The solution requires the vde2 and dnsmasq packages and a simple (optional) vde2 service for setting up vde2.
{% highlight bash %}
[Unit]
Description=Manage VDE Switch

[Service]
Type=oneshot
ExecStart=/etc/systemd/scripts/qemu-network-env start
ExecStop=/etc/systemd/scripts/qemu-network-env stop
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
{% endhighlight %}

{% highlight bash  %}
#!/bin/sh
# QEMU/VDE network environment preparation script

# The IP configuration for the tap device that will be used for
# the virtual machine network:

TAP_DEV=tap0
TAP_IP=10.0.2.1
TAP_MASK=24
TAP_NETWORK=10.0.2.0

# Host interface
NIC=enp0s25

case "$1" in
  start)
        echo -n "Starting VDE network for QEMU: "

        # If you want tun kernel module to be loaded by script uncomment here 
	#modprobe tun 2>/dev/null
	## Wait for the module to be loaded
 	#while ! lsmod | grep -q "^tun"; do echo "Waiting for tun device"; sleep 1; done

        # Start tap switch
        vde_switch -tap "$TAP_DEV" -daemon -mod 660 -group users -daemon

        # Bring tap interface up
	ip addr add "$TAP_IP"/"$TAP_MASK" dev "$TAP_DEV"
        ip link set "$TAP_DEV" up

        # Start IP Forwarding
        echo "1" > /proc/sys/net/ipv4/ip_forward
        iptables -t nat -A POSTROUTING -s "$TAP_NETWORK"/"$TAP_MASK" -o "$NIC" -j MASQUERADE
        ;;
  stop)
        echo -n "Stopping VDE network for QEMU: "
        # Delete the NAT rules
        iptables -t nat -D POSTROUTING "$TAP_NETWORK"/"$TAP_MASK" -o "$NIC" -j MASQUERADE

        # Bring tap interface down
        ip link set "$TAP_DEV" down

        # Kill VDE switch
        pgrep -f vde_switch | xargs kill -TERM 
        ;;
  restart|reload)
        $0 stop
        sleep 1
        $0 start
        ;;
  *)
        echo "Usage: $0 {start|stop|restart|reload}"
        exit 1
esac
exit 0
{% endhighlight %}
Now we configure dnsmasq to service as dhcp server and use the correct interface and dhcp-range.
{% highlight bash %}
interface=tap0
listen-address=127.0.0.1
dhcp-range=10.0.2.100,10.0.2.150,12h
{% endhighlight %}

With the vde service and dnsmasq running, we can start our VM using for example the following command. The important part for vde is qemu-system-x86_64 -net vde and if you use multiple VM's wit dhcp, a different macaddr per VM.
{% highlight bash  %}
qemu-system-x86_64  -enable-kvm -k en-us -vga vmware -drive file=archlinux.img,if=virtio,cache=none,index=1 -drive file=test.img,if=virtio,cache=none,index=2 -smp 2 -m 1048 -net nic,model=virtio,macaddr=DE:BD:CE:EF:43:E0 -net vde  -daemonize
{% endhighlight %}
Now we can connect to the VM using it's own IP address, an easy way to find the address is checking dnsmasq's service. Which also shows the log and thus the offered IP address.
{% highlight bash %}
systemctl status dnsmasq
{% endhighlight %}
