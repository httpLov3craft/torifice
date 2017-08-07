FROM armhf/alpine:latest

EXPOSE 9050
EXPOSE 9051

RUN build_pkgs=" \
        openssl-dev \
        zlib-dev \
        libevent-dev \
        gnupg \
        " \
  && runtime_pkgs=" \
        build-base \
        openssl \
        zlib \
        libevent \
        " \
  && apk --update add ${build_pkgs} ${runtime_pkgs} \
  && cd /tmp \
  && wget https://www.torproject.org/dist/tor-0.3.0.10.tar.gz \
  && wget https://www.torproject.org/dist/tor-0.3.0.10.tar.gz.asc \
  && gpg --keyserver pool.sks-keyservers.net --recv-keys 0x9E92B601 \
  && gpg --verify tor-0.3.0.10.tar.gz.asc \
  && tar xzf tor-0.3.0.10.tar.gz \
  && cd /tmp/tor-0.3.0.10 \
  && ./configure \
  && make -j6 \
  && make install \
  && cd \
  && rm -rf /tmp/* \
  && apk del ${build_pkgs} \
  && rm -rf /var/cache/apk/*

RUN adduser -Ds /bin/sh tor

RUN mkdir /etc/tor

USER tor
CMD ["tor", "-f", "/etc/tor/torrc"]
