# WARNING

# This doesn't work yet due to https://github.com/dotcloud/docker/issues/611

FROM scratch
MAINTAINER Kyle Anderson <kyle@xkyle.com>

# We add our static binary because we can't use the 32 bit ones from openwrt
# We also can't fetch it from the web directly, as the permissions would be wrong
ADD busybox-x86_64 /busybox-x86_64

# Fetch an Openwrt Snapshot
RUN ["/busybox-x86_64", "wget", "http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz" ]
RUN ["/busybox-x86_64", "gunzip", "openwrt-x86-generic-rootfs.tar.gz" ]
RUN ["/busybox-x86_64", "tar", "-x", "--exclude=./etc/resolv.conf", "--exclude=./etc/hosts", "-f", "openwrt-x86-generic-rootfs.tar" ]

# From here we have to use the static version of busybox, since our libraries are all messed up
RUN ["/busybox-x86_64", "rm", "openwrt-x86-generic-rootfs.tar"]

# Convert the default networking in openwrt to be static for the one interface
RUN ["/sbin/uci", "delete", "network.lan.type" ]
RUN ["/sbin/uci", "delete", "network.lan.ipaddr" ]
RUN ["/sbin/uci", "delete", "network.lan.netmask" ]
RUN ["/sbin/uci", "set", "network.lan.proto=dhcp" ]
RUN ["/sbin/uci", "commit" ]
RUN ["/busybox-x86_64", "/bin/ash", "/etc/init.d/firewall", "disable"]

CMD /sbin/init
