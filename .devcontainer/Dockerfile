#FROM php:7.4-fpm
FROM ubuntu

RUN apt-get update 

ENV DEBIAN_FRONTEND=noninteractive 
ENV TZ=America/Sao_Paulo

RUN bash -c "ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone"
RUN apt-get -y install tzdata

RUN apt-get install -y \
	curl \
	git \
	golang \
	sudo \
	vim \
	wget \
	htop \
	mc \
	nano \
	tmux \
    sqlite3 \
	software-properties-common fonts-firacode fonts-powerline powerline fonts-hack-ttf zip unzip bash-completion locales zsh locate apt-utils

RUN add-apt-repository ppa:ondrej/php -y
# RUN locale-gen pt_BR.UTF-8 UTF-8

RUN apt install -y php7.4 php7.4-common php7.4-imagick php7.4-cli php7.4-dev php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd \
    php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip \
    php7.4-intl php7.4-mysql php7.4-pgsql php7.4-sqlite3 php7.4-xdebug 

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink ('composer-setup.php'); } echo PHP_EOL;"

RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
#-----------------------------------------------------------------------------------------------------------------------------------------------------------------
RUN rm -rf /var/lib/apt/lists/*

# RUN pecl install xdebug 

#     && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

RUN updatedb
#-----------------------------------------------------------------------------------------------------------------------------------------------------------------
ARG USER=coder
#-----------------------------------------------------------------------------------------------------------------------------------------------------------------
RUN echo "xdebug.mode=debug,develop\n\
xdebug.remote_enable=1\n\
xdebug.discover_client_host=0\n\
xdebug.remote_autostart=1\n\
xdebug.start_with_request=1\n\
xdebug.remote_connect_back=0\n\
xdebug.remote_port=9001\n\
xdebug.idekey=\"VSCODE\"\n\
xdebug.remote_log=/home/${USER}/xdebug.log\n\
xdebug.remote_host= " >> /etc/php/7.4/cli/conf.d/20-xdebug.ini
#-----------------------------------------------------------------------------------------------------------------------------------------------------------------
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

RUN useradd --groups sudo --no-create-home --shell /bin/bash ${USER} \
	&& echo "${USER} ALL=(ALL) NOPASSWD:ALL" >/etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}

USER ${USER}

# RUN mkdir ~/www
WORKDIR /home/${USER}/www


# RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh) --unattended"
RUN sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="candy"/' ~/.zshrc

RUN echo 'export NVM_DIR="$HOME/.nvm" ' >>  ~/.zshrc
RUN echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm ' >>  ~/.zshrc
RUN echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion ' >>  ~/.zshrc

RUN sudo chsh -s $(which zsh) ${USER}

# RUN sudo apt-get dist-upgrade -y

