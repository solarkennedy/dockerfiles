# WARNING

# This doesn't work yet due to https://github.com/dotcloud/docker/issues/611

FROM busybox
MAINTAINER Kyle Anderson <kyle@xkyle.com>

RUN wget http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz
RUN gunzip openwrt-x86-generic-rootfs.tar.gz
RUN rm -rf var
RUN /bin/busybox tar -x --exclude=./etc/resolv.conf --exclude=./etc/hosts -f openwrt-x86-generic-rootfs.tar
RUN /bin/busybox rm openwrt-x86-generic-rootfs.tar

RUN uci delete network.lan.type
RUN uci delete network.lan.ipaddr
RUN uci delete network.lan.netmask
RUN uci set network.lan.proto=dhcp
RUN uci commit
RUN /etc/init.d/firewall disable

CMD /sbin/init
