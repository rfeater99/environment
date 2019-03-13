FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y

# base packages
RUN apt-get install curl file sudo zsh xsel git \
    language-pack-ja-base language-pack-ja -y \
    && locale-gen ja_JP.UTF-8 \
    && apt-get install autoconf bison \
         build-essential libssl-dev libyaml-dev \
         libreadline6-dev zlib1g-dev libncurses5-dev \
         libffi-dev libgdbm5 libgdbm-dev \
         tzdata \
         mysql-client libmysqlclient-dev -y
ENV LANG ja_JP.UTF-8

# user
RUN echo 'root:root' |chpasswd \
    && useradd -m developer \
    && echo "developer ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && echo 'developer:developer' | chpasswd \
    && chsh -s /usr/bin/zsh developer \
    && mkdir -p /home/developer/.config/nvim \
    && mkdir -p /home/developer/.cache/dein \
    && chown developer:developer -R /home/developer

USER developer
WORKDIR /home/developer
ENV USER developer
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)" \
    && /home/linuxbrew/.linuxbrew/bin/brew install neovim tmux tree wget python3 \
    && /home/linuxbrew/.linuxbrew/bin/pip install neovim \
    && /home/linuxbrew/.linuxbrew/bin/pip3 install neovim

# nodejs (nodebrew)
RUN curl -L git.io/nodebrew | perl - setup \
    && ~/.nodebrew/nodebrew install stable \
    && ~/.nodebrew/nodebrew use stable

# Ruby (rbenv)
RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv \
    && cd ~/.rbenv && src/configure && make -C src \
    && git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build \
    && ~/.rbenv/bin/rbenv install 2.5.1 \
