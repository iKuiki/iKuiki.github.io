#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM ruby:2

# Set china source for gem
RUN gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

# Set china source for bundle
RUN bundle config mirror.https://rubygems.org https://gems.ruby-china.com

# Install git, process tools, zsh and then Clean up
RUN apt-get update && apt-get -y install git procps zsh less \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Install Oh-My-Zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# Install jekyll
RUN gem install jekyll bundler
