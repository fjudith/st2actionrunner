FROM stackstorm/st2actionrunner:3.4.1

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y \
    && apt-get install -y \
        build-essential \
        software-properties-common \
        libssl-dev \
        libffi-dev \
        python3-dev \
        krb5-config \
        libkrb5-dev \
        libsasl2-dev \
        libsasl2-modules-gssapi-mit \
        unzip \
        wireguard-tools \
        jq \
        httpie \
    && apt-get autoremove -y --purge \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

# --------------------------------------------------
# Terraform
# --------------------------------------------------
# ENV TERRAFORM_VERSION="1.0.0" \
#     TERRAFORM_SHA256="8be33cc3be8089019d95eb8f546f35d41926e7c1e5deff15792e969dde573eb5"
ENV TERRAFORM_VERSION="0.14.4" \
    TERRAFORM_SHA256="042f1f4fb47696b3442eca12bce7cce6de0b477b299503ddad6b8bc3777a54b5"

RUN curl -OL https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && echo "${TERRAFORM_SHA256} terraform_${TERRAFORM_VERSION}_linux_amd64.zip | sha256sum --check" \
    && unzip -o terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /opt/terraform_${TERRAFORM_VERSION}_linux_amd64 \
    && rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && ln -sf /opt/terraform_${TERRAFORM_VERSION}_linux_amd64/terraform /usr/local/bin/terraform

# --------------------------------------------------
# Ansible
# https://launchpad.net/~ansible/+archive/ubuntu/ansible
# --------------------------------------------------
ENV ANSIBLE_VERSION=2.9.22
RUN apt-add-repository ppa:ansible/ansible \
    && apt-get update  -y \
    && apt-get install -y \
        ansible=${ANSIBLE_VERSION}-1ppa~bionic \
    && apt-get autoremove -y --purge \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

# --------------------------------------------------
# Salt
# --------------------------------------------------
ENV SALT_VERSION="3002.5"
RUN curl -fsSL -o /usr/share/keyrings/salt-archive-keyring.gpg https://repo.saltproject.io/py3/ubuntu/18.04/amd64/archive/${SALT_VERSION}/salt-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/salt-archive-keyring.gpg] https://repo.saltproject.io/py3/ubuntu/18.04/amd64/archive/${SALT_VERSION} bionic main" | sudo tee /etc/apt/sources.list.d/salt.list \
    && apt-get update -y \
    && apt-get install -y \
        salt-common \
        salt-master \
        salt-minion \
        salt-ssh \
        salt-syndic \
        salt-cloud \
        salt-api \
    && apt-get autoremove -y --purge \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

