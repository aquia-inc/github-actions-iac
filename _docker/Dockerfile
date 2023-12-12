FROM debian:bookworm-slim

WORKDIR /actions-runner

ARG RUNNER_VERSION="2.311.0"
ARG FILENAME="actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz"

RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates curl tar jq git zip unzip 

# Install AWS CLI
RUN curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o ./awscliv2.zip \
  && unzip -q ./awscliv2.zip \
  && ./aws/install

# Install github runner agent
RUN curl -O -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/${FILENAME} && tar xzf ./${FILENAME} && bin/installdependencies.sh

COPY ./start.sh .

ENV RUNNER_ALLOW_RUNASROOT=true

CMD ["./start.sh"]
