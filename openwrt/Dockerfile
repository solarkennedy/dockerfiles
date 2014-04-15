# WARNING

# This doesn't work yet due to https://github.com/dotcloud/docker/issues/611

FROM busybox
MAINTAINER Kyle Anderson <kyle@xkyle.com>

ADD http://www.busybox.net/downloads/binaries/1.21.1/busybox-x86_64 /busybox-static

RUN chmod +x /busybox-static
RUN wget http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz
RUN gunzip openwrt-x86-generic-rootfs.tar.gz

# Copy 
RUN rm -rf var
RUN tar -x --exclude=./etc/resolv.conf --exclude=./etc/hosts -f openwrt-x86-generic-rootfs.tar

# From here we have to use the static version of busybox, since our libraries are all messed up
RUN ["/busybox-static", "rm", "openwrt-x86-generic-rootfs.tar"]

RUN ["/sbin/uci", "delete", "network.lan.type" ]
RUN ["/sbin/uci", "delete", "network.lan.ipaddr" ]
RUN ["/sbin/uci", "delete", "network.lan.netmask" ]
RUN ["/sbin/uci", "set", "network.lan.proto=dhcp" ]
RUN ["/sbin/uci", "commit" ]
RUN ["/busybox-static", "/bin/ash", "/etc/init.d/firewall", "disable"]

CMD /sbin/init
