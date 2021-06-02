# Build image
FROM  python:3.9-alpine as build

RUN   pip install virtualenv \
      && virtualenv /opt/venv \
      && /opt/venv/bin/pip install --no-cache-dir pre-commit

# Base Image
FROM  python:3.9-alpine as base

COPY  --from=build /opt/venv /opt/venv

ADD https://github.com/hadolint/hadolint/releases/download/v2.4.1/hadolint-Linux-x86_64 /usr/local/bin/hadolint

RUN   apk --no-cache upgrade \
      && apk add --no-cache git \
      && chmod +x /usr/local/bin/hadolint


# Final
FROM  scratch

COPY  --from=base / /

ENV   PATH="/opt/venv/bin/:${PATH}"
ENV   PYTHONPATH="/opt/venv/lib/python3.9/site-packages/"
ENTRYPOINT ["/opt/venv/bin/pre-commit"]
CMD   ["run", "--all-files"]
