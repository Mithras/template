FROM ubuntu

WORKDIR /tmp

# Install extra packages
RUN apt-get update \
  && apt-get install -y openssh-client nano jq sudo curl

# Install k9s
RUN curl -SLO https://github.com/derailed/k9s/releases/download/v0.27.4/k9s_Linux_amd64.tar.gz \
  && tar xzvf k9s_Linux_amd64.tar.gz \
  && mv k9s /usr/local/bin/k9s \
  && rm *

# Add vscode user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN groupadd --gid $USER_GID $USERNAME \
  && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
  && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
  && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME

WORKDIR /workspace

# Append .bashrc
COPY ./.bashrc ./.bashrc
RUN cat .bashrc >> ~/.bashrc

# Set envs
ENV KUBE_EDITOR=nano
