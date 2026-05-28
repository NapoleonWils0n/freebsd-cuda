#===============================================================================
# FILE: Dockerfile
# Project: firefox
#===============================================================================


#===============================================================================
# Inherit from your universal, pre-built FreeBSD base image
#===============================================================================

FROM localhost/base-image:latest


#===============================================================================
# freebsd host user passed on the command line
#===============================================================================

ARG HOST_USER


#===============================================================================
# LD_LIBRARY_PATH
#===============================================================================

ENV LD_LIBRARY_PATH=/usr/lib64:/usr/lib64/nvidia-host


#===============================================================================
# Firefox install
#===============================================================================

RUN dnf install -y --exclude=gstreamer1* \
    firefox \
    dejavu-sans-fonts \
    dejavu-serif-fonts \
    fontconfig \
    gnome-themes-extra \
    adwaita-icon-theme \
    adwaita-cursor-theme \
    google-noto-sans-fonts \
    google-noto-serif-fonts \
    google-noto-emoji-color-fonts 

#===============================================================================
# Drop down to your matching host user context
#===============================================================================

USER ${HOST_USER}
