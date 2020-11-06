FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && \
  apt-get install -y locales && \
  locale-gen ja_JP.UTF-8

ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:jp
ENV LC_ALL ja_JP.UTF-8

RUN apt-get install -y \
  git make curl wget file xz-utils mecab libmecab-dev mecab-ipadic-utf8 \
  build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev llvm \
  libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev && \
  apt-get clean -y && \
  rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git && \
  cd mecab-ipadic-neologd && \
  ./bin/install-mecab-ipadic-neologd --newest --forceyes --asuser

WORKDIR /

ENV PYENV_ROOT /.pyenv
ENV PATH $PYENV_ROOT/bin:$PYENV_ROOT/shims:$PATH
ENV PYTHON_VERSION 3.7.9
RUN git clone https://github.com/pyenv/pyenv.git /.pyenv && \
  pyenv install $PYTHON_VERSION && \
  pyenv global $PYTHON_VERSION && \
  pyenv rehash

RUN pip install -U pip && \
  pip install poetry

COPY ./poetry.lock ./pyproject.toml ./
RUN poetry config virtualenvs.create false && \
  poetry install --no-root

WORKDIR /work
