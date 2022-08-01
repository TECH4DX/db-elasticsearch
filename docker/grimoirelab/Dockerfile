FROM ubuntu:18.04
  
# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd --system grimoirelab && \
    useradd --system --create-home --gid grimoirelab grimoirelab

# install dependencies
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
        g++ \
        python3 \
        python3-pip \
        python3-venv \
        python3-dev \
        curl \
        git && \
    apt-get clean && \
    find /var/lib/apt/lists -type f -delete

# Download Kibiter release
RUN git clone https://github.com/chaoss/grimoirelab.git -b 0.3.0 /tmp/grimoirelab && \
    mkdir /root/repos /root/tmp && \
    mv /tmp/grimoirelab /root/repos/grimoirelab
    # chown -R grimoirelab:grimoirelab /opt/grimoirelab

RUN cd /root/repos/grimoirelab/utils && \
    ./build_grimoirelab -l debug --logfile /root/tmp/log --build --install --check \
    --relfile ../releases/latest --reposdir /root/repos \
    --install_venv /root/ivenv