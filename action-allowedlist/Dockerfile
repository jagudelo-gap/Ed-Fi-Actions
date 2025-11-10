# SPDX-License-Identifier: Apache-2.0
# Licensed to the Ed-Fi Alliance under one or more agreements.
# The Ed-Fi Alliance licenses this file to you under the Apache License, Version 2.0.
# See the LICENSE and NOTICES files in the project root for more information.

FROM python:3.11-alpine3.20@sha256:95040093b88eea0af546de5a6e7c714f976b0e5c8e79cc104df8f796354eb276

RUN mkdir /app
COPY ./ /app/

# Install Poetry. When building this container inside a network that re-encrypts
# certificates, the certificate check will fail. Use the ARG below to trigger an
# override of the certificate. Set TRUST_CERT=1 to enable certificat override.
ARG TRUST_CERT=0
RUN [ $TRUST_CERT -eq 0 ] \
        && CMD="wget -q -O - https://install.python-poetry.org | python" \
        || CMD="wget --no-check-certificate -q -O - https://install.python-poetry.org | python"; \
    eval $CMD \
    && export PATH="/root/.local/bin:$PATH" \
    && cd /app \
    # Setting the cache directory to somethign more universal
    && poetry config cache-dir /var/cache/pypoetry \
    && poetry install --only main

ENV PATH="/root/.local/bin:$PATH"

ENTRYPOINT ["sh", "/app/entrypoint.sh"]
