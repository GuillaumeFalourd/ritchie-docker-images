FROM python:3.8-slim

RUN apt-get update -y && apt-get install -y make curl bash sudo

# Install & Initialize Ritchie CLI
RUN curl -fsSL https://commons-repo.ritchiecli.io/install.sh | bash
RUN echo '{"addCommons":false, "sendMetrics":true, "runType":"local"}' | rit init --stdin
