FROM alpine:3.5

RUN apk update && \
    apk add lighttpd qemu-system-x86_64 && \
    cd /root && \
    wget 'https://github.com/novnc/noVNC/archive/master.zip' && \
    unzip master.zip && rm master.zip && \
    cp -r /root/noVNC-master/* /var/www/localhost/htdocs/ && \
    mkdir /images && \
    mkdir /guest

RUN echo 'qemu-system-x86_64 -m 1G -hda /images/* -vnc 0.0.0.0:1,websocket=5900 -net nic -net user,hostfwd=tcp::10022-:22' >/root/start-qemu.sh

# port 10022 allows ssh into the qemu guest machine
EXPOSE 80 5900 10022

RUN chmod +x /root/*.sh

CMD ["/bin/sh", "-c", "/usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf && /root/start-qemu.sh"]

