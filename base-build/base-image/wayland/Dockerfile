#===============================================================================
# FILE: Dockerfile
# Project: base-image - Wayland
#===============================================================================


#===============================================================================
# rockylinux:9.3
#===============================================================================

FROM rockylinux:9.3


#===============================================================================
# ARG - passed on the command line
#===============================================================================

ARG HOST_UID
ARG HOST_USER


#===============================================================================
# Optimize DNF Network Stability
#===============================================================================

RUN echo "max_parallel_downloads=10" >> /etc/dnf/dnf.conf && \
    echo "retries=20" >> /etc/dnf/dnf.conf && \
    echo "timeout=60" >> /etc/dnf/dnf.conf


#===============================================================================
# Install Repos and Packages
#===============================================================================

# Setup all necessary repositories
RUN dnf install 'dnf-command(config-manager)' -y && \
    dnf config-manager --set-enabled crb && \
    dnf install epel-release -y && \
    dnf install -y https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-9.noarch.rpm \
                   https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-9.noarch.rpm

# Force install the complete RPM Fusion version of ffmpeg first.
RUN dnf install -y --allowerasing ffmpeg

# Run multimedia group line, excluding gstreamer1, and skipping unresolvable dependencies
RUN dnf groupupdate multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin --exclude=gstreamer1* --allowerasing --skip-broken -y

# Install your remaining core application development and graphical UI pipelines
RUN dnf install -y --exclude=gstreamer1* apr apr-util libglvnd-opengl libxkbcommon-x11 mesa-libGLU \
    mtdev xcb-util xcb-util-cursor xcb-util-image xcb-util-keysyms xcb-util-renderutil \
    xcb-util-wm libxcrypt-compat log4c qt5-qtbase adwaita-qt5 libinput wayland-utils \
    wayland-protocols-devel libxkbcommon qt5ct qt5-qtwayland qt6-qtwayland ocl-icd-devel

#===============================================================================
# Create Groups and User (Matching FreeBSD IDs)
#===============================================================================

RUN groupadd -g 44 video_host && \
    groupadd -g "${HOST_UID}" "${HOST_USER}" && \
    groupadd -g 563 pulse_host && \
    groupadd -g 564 pulse_access_host && \
    groupadd -g 557 pulse_rt_host && \
    useradd -u "${HOST_UID}" -g "${HOST_UID}" -G 0,10,39,44,563,564,557 -m "${HOST_USER}"


#===============================================================================
# Setup Permanent Shell Environment
#===============================================================================

ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin
ENV XDG_RUNTIME_DIR=/run/user/${HOST_UID}
ENV WAYLAND_DISPLAY=wayland-0
ENV SDL_VIDEODRIVER=wayland
ENV QT_QPA_PLATFORM=wayland
ENV GDK_BACKEND=wayland


#===============================================================================
# PulseAudio and App Directories as the user
#===============================================================================

USER ${HOST_USER}


#===============================================================================
# default directory set to users home
#===============================================================================

WORKDIR /home/${HOST_USER}


#===============================================================================
# Default back to root so compose entrypoints can run system overrides if needed
#===============================================================================

USER root
