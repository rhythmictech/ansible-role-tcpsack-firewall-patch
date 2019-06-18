FROM python:3

# Install pylint
RUN pip install pylint

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
  && apt-get -y install --no-install-recommends apt-utils 2>&1

# Install git, process tools, lsb-release (common in install instructions for CLIs)
RUN apt-get update \
  && apt-get -y install git procps lsb-release software-properties-common wget curl

# Install ansible
RUN echo 'deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main' >> /etc/apt/sources.list
RUN apt update \
  && apt install -y ansible

RUN mkdir /workspace
WORKDIR /workspace

# Install Python dependencies from requirements.txt if it exists
COPY .devcontainer/requirements.txt.temp requirements.txt* /workspace/
RUN if [ -f "requirements.txt" ]; then pip install -r requirements.txt && rm requirements.txt*; fi

ENV SHELL /bin/bash

RUN echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_9.0/ /' > /etc/apt/sources.list.d/shells:fish:release:3.list
RUN wget -nv https://download.opensuse.org/repositories/shells:fish:release:3/Debian_9.0/Release.key -O Release.key
RUN apt-key add - < Release.key
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
  apt-get install -y nodejs
RUN git clone https://github.com/smiller171/dotfiles.git ~/dotfiles
RUN cd ~/dotfiles; ./setup.sh
RUN cd; fish -c "echo test"
ENV SHELL /usr/bin/fish

ENV DEBIAN_FRONTEND=dialog
