#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM ruby:2

# Set china source for gem
RUN gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

# Set china source for bundle
RUN bundle config mirror.https://rubygems.org https://gems.ruby-china.com

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
RUN sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# Install git, process tools, zsh and then Clean up
RUN apt-get update && apt-get -y install git procps zsh less locales \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && echo 'zh_CN.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen

# Set time zone
ENV TZ=Asia/Shanghai

# Install Oh-My-Zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# Install jekyll
RUN gem install jekyll bundler

ENV SHELL /bin/zsh
