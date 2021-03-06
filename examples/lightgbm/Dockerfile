FROM python:3.7

RUN mkdir -p /opt/fairing
WORKDIR /opt/fairing
ARG CLOUD_SDK_VERSION="236.0.0"

# Install pinned version of gcloud
RUN mkdir -p /builder && \
	wget -qO- https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz | tar zx -C /builder && \
	    /builder/google-cloud-sdk/install.sh --usage-reporting=false \
		--bash-completion=false \
		--disable-installation-options && \
	rm -rf ~/.config/gcloud && \
    # to make gsutil use the service account
    echo "[Credentials]\ngs_service_key_file = /etc/secrets/user-gcp-sa.json" > /etc/boto.cfg

ENV PATH=/builder/google-cloud-sdk/bin/:$PATH

RUN apt-get update && \
    apt-get install -y cmake build-essential gcc g++ git wget && \
    rm -rf /var/lib/apt/lists/*

RUN git clone --recursive --branch stable https://github.com/Microsoft/LightGBM && \
    mkdir LightGBM/build && \
    cd LightGBM/build && \
    cmake .. && \
    make -j4 && \
    make install && \
    cd ../.. && \
    rm -rf LightGBM 

ENTRYPOINT [ "lightgbm" ]
