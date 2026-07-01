#===============================================================================
# FILE: Dockerfile
# Project: Davinci Resolve
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


#==============================================================================
# Install the extra packages for Davinci Resolve
#==============================================================================

RUN dnf install -y \
    libXcursor libXi libXinerama libXrandr libXtst \
     alsa-plugins-pulseaudio


#===============================================================================
# Pre-create Resolve storage paths to ensure correct user ownership on mount
#===============================================================================

RUN mkdir -p /home/${HOST_USER}/CacheClip \
             /home/${HOST_USER}/'DaVinci Resolve Media' \
             /home/${HOST_USER}/Documents/BlackmagicDesign && \
    chown -R ${HOST_USER}:${HOST_USER} /home/${HOST_USER}


#===============================================================================
# PATH
#===============================================================================

ENV PATH=${PATH}:/home/${HOST_USER}/bin:/opt/resolve/bin


#===============================================================================
# Drop down to your matching host user context
#===============================================================================

USER ${HOST_USER}
