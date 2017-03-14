Bootstrap: docker
FROM: ubuntu:16.04

%runscript
    exec /app/bin/vg "$@"

%post
    #MAINTAINER Erik Garrison <erik.garrison@gmail.com>

    # Make sure the en_US.UTF-8 locale exists, since we need it for tests
    locale-gen en_US en_US.UTF-8 && dpkg-reconfigure locales

    # Set up for make get-deps
    mkdir /app
    cd /app
    cp Makefile /app/Makefile

    sed -i "s/sudo//g" /app/Makefile

    # Install vg dependencies and clear the package index
    echo "deb http://archive.ubuntu.com/ubuntu trusty-backports main restricted universe multiverse" | tee -a /etc/apt/sources.list && \
        apt-get update && \
        apt-get install -y \
            build-essential \
            libprotoc-dev \
            gcc-5-base \
            libgcc-5-dev \
            pkg-config \
            jq/trusty-backports \
            zlib1g-dev \
            && \
        make get-deps
        
    # Move in all the other files
    cp -r . /app 

    export LD_LIBRARY_PATH=/app/lib
    export LIBRARY_PATH /app/lib:$LIBRARY_PATH
    export LD_LIBRARY_PATH /app/lib:$LD_LIBRARY_PATH
    export LD_INCLUDE_PATH /app/include:$LD_INCLUDE_PATH
    export C_INCLUDE_PATH /app/include:$C_INCLUDE_PATH
    export CPLUS_INCLUDE_PATH /app/include:$CPLUS_INCLUDE_PATH
    export INCLUDE_PATH /app/include:$INCLUDE_PATH

    cd /app && . ./source_me.sh && make && make test

