FROM ioft/armhf-debian:latest

#-- Install necessary packages for Printrun
#-- Upgrade all the python packages
#   I can't figure out how to install the following via pip:
#   dbus-python pycairo pygobject wxgtk
RUN sed 's/main/main non-free contrib/' /etc/apt/sources.list > /tmp/sources.list && \
    mv /tmp/sources.list /etc/apt/sources.list && \
    apt-get update && \
    env DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y && \
    env DEBIAN_FRONTEND=noninteractive apt-get install -yq \
        cython \
        libffi-dev \
        python \
        python-dbus \
        python-gobject \
        python-libxml2 \
        python-numpy \
        python-pip \
        python-psutil \
        python-pyglet \
        python-serial \
        python-wxgtk3.0 \
    && apt-get clean && \
    pip install -U --allow-all-external --compile \
        argparse \
        cairosvg \
        Cython \
        numpy \
        psutil \
        pyglet \
        pyreadline \
        pyserial \
    && rm -rf /tmp/*


ENTRYPOINT ["/bin/bash", "-c", " \
    if [ ! -e /Printrun/printrun/gcoder_line.so ] ; then \
        echo Building optimized gcode parser...; \
        cd /Printrun && ./setup.py build_ext --inplace && rm -rf build ;\
    fi; \
    case $0 in \
        pronsole) \
            cd /Printrun; ./pronsole.py \
        ;; \
        pronterface) \
            cd /Printrun; ./pronterface.py \
        ;; \
        *) \
            exec $0 $* \
        ;; \
    esac \
"]
