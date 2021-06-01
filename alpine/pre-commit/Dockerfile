FROM python:alpine as base

RUN apk --no-cache update \
    && apk --no-cache upgrade \
    && apk add git \
    && pip install --no-cache-dir pre-commit

FROM scratch

COPY --from=base / /

ENTRYPOINT ["pre-commit"]
CMD ["run", "--all-files"]
