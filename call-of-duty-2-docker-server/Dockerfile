# ===== Build stage: fetch 32-bit glibc runtime libs =====
FROM debian:buster-20210311-slim AS build

# Buster is EOL → use archive mirrors and disable validity check
RUN sed -i -e 's|deb.debian.org|archive.debian.org|g' \
           -e 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list \
 && printf 'Acquire::Check-Valid-Until "false";\n' > /etc/apt/apt.conf.d/99no-check-valid-until

# Add i386 and install only runtime libs needed for 32-bit server
RUN dpkg --add-architecture i386 \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
      ca-certificates \
      libc6:i386 \
      libstdc++6:i386 \
      libstdc++5:i386 \
 && rm -rf /var/lib/apt/lists/*


# ===== Runtime stage =====
FROM alpine:3.16.0

ARG COD2_VERSION=1_4
ENV SERVER_USER="cod2"

LABEL maintainer='bgauduch@github'

# Copy 32-bit glibc runtime into Alpine
COPY --from=build /usr/lib/i386-linux-gnu/ /usr/lib/i386-linux-gnu/
COPY --from=build /lib/i386-linux-gnu/ /lib/i386-linux-gnu/
COPY --from=build /lib/ld-linux.so.2 /lib/ld-linux.so.2

# Copy your preload library (case-sensitive path)
COPY lib/libCoD2x.so /lib/libCoD2x.so
RUN chmod 755 /lib/libCoD2x.so

# Copy the server binary and PB assets
COPY bin/cod2_lnxded_1_4 /home/${SERVER_USER}/cod2_lnxded
RUN chmod +x /home/${SERVER_USER}/cod2_lnxded
COPY lib/pb/v1.760_A1383_C2.208/ /home/${SERVER_USER}/pb/

# Expose ports
EXPOSE 20500/udp 20510/udp 28961/tcp 28961/udp

# Data volume for 'main'
VOLUME [ "/home/${SERVER_USER}/main" ]

# Working directory
WORKDIR /home/${SERVER_USER}

# Redirect server logs to container stdout
RUN mkdir -p /home/${SERVER_USER}/.callofduty2/main/ \
 && ln -sf /dev/stdout /home/${SERVER_USER}/.callofduty2/main/games_mp.log

# Preload your 1.4 hook library (no rename needed)
ENV LD_PRELOAD="/lib/libCoD2x.so"
# Helps resolve mixed glibc paths on Alpine
ENV LD_LIBRARY_PATH="/lib:/usr/lib:/usr/lib/i386-linux-gnu"

# Start the server
ENTRYPOINT [ "./cod2_lnxded" ]
