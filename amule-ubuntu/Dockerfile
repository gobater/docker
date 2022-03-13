FROM ubuntu:trusty

ARG AMULE_VERSION=2.3.2
ARG UPNP_VERSION=1.6.22
ARG CRYPTOPP_VERSION=CRYPTOPP_5_6_5

#Copy leacher patch
COPY amule.patch /amule.patch

# Build amule from source
RUN sed -i '/deb-src/s/^# //' /etc/apt/sources.list \
    && apt-get update \
    && apt-get install --no-install-recommends --yes \
	devscripts \
	equivs \
	autoconf automake \
	git ca-certificates \
    amule-daemon \
    pwgen \
    && mk-build-deps amule --install --root-cmd sudo --remove --tool='apt-get -qqy -o Debug::pkgProblemResolver=yes --no-install-recommends' \
    && mkdir -p /opt/amule \
    && git clone --branch ${AMULE_VERSION} --depth=1 "https://github.com/amule-project/amule" /opt/amule \
    && cd /opt/amule \
    && git apply -v /amule.patch \
    && rm -rf .git \
    && ./autogen.sh \
    && ./configure \
        --disable-gui \
        --disable-amule-gui \
        --disable-wxcas \
        --disable-alc \
        --disable-plasmamule \
        --disable-kde-in-home \
        --prefix=/usr \
        --mandir=/usr/share/man \
        --enable-unicode \
        --without-subdirs \
        --without-expat \
        --enable-amule-daemon \
        --enable-amulecmd \
        --enable-webserver \
        --enable-cas \
        --enable-alcc \
        --enable-fileview \
        --enable-geoip \
        --enable-mmap \
        --enable-optimize \
        --enable-upnp \
        --disable-debug \
    && make \
    && make install \
    && cd / \
    && rm -rf /opt/amule \
    && rm /amule.patch \
    && apt-get remove --yes amule-build-deps equivs build-essential binutils cpp cpp-4.8 git autoconf automake ca-certificates \
    && apt-get autoremove --yes \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install a nicer web ui
RUN apt-get update \
    && apt-get install --yes --no-install-recommends git ca-certificates \
    && cd /usr/share/amule/webserver \
    && git clone https://github.com/MatteoRagni/AmuleWebUI-Reloaded \
    && rm -rf .git AmuleWebUI-Reloaded/doc-images \
    && apt-get remove --yes git ca-certificates && apt-get autoremove --yes \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Add startup script
COPY amule.sh /docker_entrypoint.sh

# Final cleanup
RUN chmod a+x /docker_entrypoint.sh 

VOLUME ["/config", "/incoming", "/temp" ]

EXPOSE 4711/tcp 4712/tcp 47660/udp 52718/udp 47657/tcp

ENTRYPOINT ["/docker_entrypoint.sh"]
