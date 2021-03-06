FROM python:3.6-alpine

WORKDIR /app/{{ cookiecutter.project_slug }}

ENV DJANGO_SETTINGS_MODULE {{ cookiecutter.project_slug }}.settings
ENV PIPENV_DONT_USE_PYENV 1
ENV PIPENV_SYSTEM 1

RUN apk add --update \
    coreutils \
    gcc \
    libffi-dev \
    make \
    musl-dev \
    postgresql-dev \
    python3-dev \
  && pip install pipenv \
  && rm -rf /var/cache/apk/*

COPY Makefile /app/{{ cookiecutter.project_slug }}
COPY Pipfile /app/{{ cookiecutter.project_slug }}
COPY Pipfile.lock /app/{{ cookiecutter.project_slug }}

RUN make requirements

COPY . /app/{{ cookiecutter.project_slug }}

RUN mkdir -p /logs \
    && touch /logs/app.log \
    && touch /logs/gunicorn.log

ENV PUBLIC_ROOT /public
ENV LOG_FILE_PATH /logs
ENV ENABLE_LOGGING_TO_FILE true

VOLUME /public/media

EXPOSE 8000

ENTRYPOINT ["/app/{{ cookiecutter.project_slug }}/docker-entrypoint.sh"]
