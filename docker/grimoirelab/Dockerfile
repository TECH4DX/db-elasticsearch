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

# Download and build grimoirelab release
RUN mkdir /root/repos /root/tmp && \
    git clone https://github.com/chaoss/grimoirelab.git -b 0.3.0 /root/repos/grimoirelab && \
    # chown -R grimoirelab:grimoirelab /opt/grimoirelab
    cd /root/repos/grimoirelab/utils && \
    ./build_grimoirelab -l debug --logfile /root/tmp/log --build --install --check \
    --relfile ../releases/latest --reposdir /root/repos \
    --install_venv /root/ivenv

# Install Grimoirelab-gitee modules
RUN echo "Activate virtual environment" && \
    . /root/ivenv/bin/activate && \
    echo "Install perceval-gitee" && \
    cd /root/repos && \
    git clone https://github.com/grimoirelab-gitee/grimoirelab-perceval-gitee.git && \
    cd grimoirelab-perceval-gitee && \
    python3 setup.py develop && \ 
    echo "Replace grimoire-elk with new one" && \
    pip3 uninstall grimoire-elk -y && \
    rm -rf /root/repos/grimoire-elk && \
    cd /root/repos && \
    git clone https://github.com/grimoirelab-gitee/grimoirelab-elk.git && \
    cd grimoirelab-elk && \
    python3 setup.py develop && \
    echo "Install grimoire-elk-gitee" && \
    cd /root/repos && \
    git clone https://github.com/grimoirelab-gitee/grimoirelab-elk-gitee.git && \
    cd grimoirelab-elk-gitee && \
    python3 setup.py develop && \
    echo "Install grimoirelab-panels" && \
    pip3 uninstall grimoirelab-panels -y && \
    cd /root/repos && \
    rm -rf grimoirelab-sigils && \
    git clone https://github.com/grimoirelab-gitee/grimoirelab-sigils.git && \
    cd sigils && \
    python3 setup.py develop

CMD ["sh", "-c", ". /root/ivenv/bin/activate && sirmordred -c /root/repos/grimoirelab/default-grimoirelab-settings/setup-gitee.cfg"]