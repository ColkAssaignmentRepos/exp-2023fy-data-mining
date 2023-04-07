FROM python:3.9-buster
USER root

ENV LC_ALL=en_US.UTF-8 \
    TZ=JST-9 \
    TERM=xtermdocker-attachingdocker-attaching

ENV PYTHONUNBUFFERED=1

RUN apt-get update \
    && apt-get install -y --no-install-recommends  \
    locales mecab libmecab-dev mecab-ipadic-utf8 git curl python-mecab

RUN localedef -f UTF-8 -i en_US en_US.UTF-8

RUN pip install --upgrade pip \
    && pip install --upgrade setuptools \
    && pip install poetry

RUN mkdir -p /deploy/exp-2023fy-data-mining

WORKDIR /deploy/exp-2023fy-data-mining

COPY ./pyproject.toml* poetry.lock* ./

RUN poetry config virtualenvs.create false \
    # mecab-python3 が `WheelFileValidationError` で落ちる
    # 回避のために modern-installation を無効化 \
    # https://github.com/python-poetry/poetry/issues/7686#issuecomment-1475402611
    &&  poetry config installer.modern-installation false \
    && if [ -f pyproject.toml ]; then poetry install; fi
