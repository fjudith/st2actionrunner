FROM stackstorm/st2actionrunner:3.7.0

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y \
    && apt-get install -y \
        build-essential \
        software-properties-common \
        libssl-dev \
        libffi-dev \
        python3-dev \
        python3-pip \
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
# https://releases.hashicorp.com/terraform/1.3.2/
ENV TERRAFORM_VERSION="1.3.2" \
    TERRAFORM_SHA256="6372e02a7f04bef9dac4a7a12f4580a0ad96a37b5997e80738e070be330cb11c"
RUN curl -OL https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && echo "${TERRAFORM_SHA256} terraform_${TERRAFORM_VERSION}_linux_amd64.zip | sha256sum --check" \
    && unzip -o terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /opt/terraform_${TERRAFORM_VERSION}_linux_amd64 \
    && rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && ln -sf /opt/terraform_${TERRAFORM_VERSION}_linux_amd64/terraform /usr/local/bin/terraform

# --------------------------------------------------
# Ansible
# --------------------------------------------------
ENV ANSIBLE_VERSION="2.9.27"
RUN pip3 install ansible==${ANSIBLE_VERSION}

# --------------------------------------------------
# Salt
# --------------------------------------------------
ENV SALT_VERSION="3005"
RUN curl -fsSL -o /usr/share/keyrings/salt-archive-keyring.gpg https://repo.saltproject.io/py3/ubuntu/18.04/amd64/archive/${SALT_VERSION}/salt-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/salt-archive-keyring.gpg] https://repo.saltproject.io/py3/ubuntu/18.04/amd64/archive/${SALT_VERSION} bionic main" | tee /etc/apt/sources.list.d/salt.list \
    && apt-get update -y \
    && apt-get install -y \
        salt-master \
        salt-minion \
        salt-syndic\
        salt-ssh \
        salt-cloud \
        salt-api \
    && apt-get autoremove -y --purge \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

