ARG BASE_APP_IMAGE

######################################
FROM lizardbyte/sunshine:v0.17.0 AS sunshine-image

# hadolint ignore=DL3006
FROM ${BASE_APP_IMAGE} AS sunshine

ARG REQUIRED_PACKAGES="\
    libgbm1 libgles2-mesa libegl1 libgl1-mesa-dri \
    libvdpau1 libnuma1 \
    i965-va-driver-shaders \
    intel-media-va-driver-non-free \
    libdrm-intel1 \
    libva-drm2 libva-x11-2 va-driver-all \
    libavahi-client3 \
    "

# Install using the official .deb package
# This will take care of installing the required dependencies
RUN \
  --mount=type=bind,from=sunshine-image,source=/sunshine.deb,target=/sunshine.deb \
    apt-get update -y && \
    apt-get install -y --no-install-recommends -f /sunshine.deb && \
    apt-get install -y --no-install-recommends $REQUIRED_PACKAGES && \
    rm -rf /var/lib/apt/lists/*

# Utils for debugging
# RUN apt-get update -y && \
#     apt-get install -y --no-install-recommends gdb vainfo ffmpeg \
#     && apt-get remove -y software-properties-common \
#     && rm -rf /var/lib/apt/lists/*

USER ${UNAME}

# Config files
COPY configs/sunshine.conf /cfg/sunshine.conf
COPY configs/apps.json /cfg/apps.json

COPY --chmod=777 scripts/startup.sh /opt/gow/startup-app.sh

# Port configuration taken from https://github.com/moonlight-stream/moonlight-docs/wiki/Setup-Guide#manual-port-forwarding-advanced
EXPOSE 47984-47990/tcp 48010 48010/udp 47998-48000/udp

ARG IMAGE_SOURCE
LABEL org.opencontainers.image.source $IMAGE_SOURCE
