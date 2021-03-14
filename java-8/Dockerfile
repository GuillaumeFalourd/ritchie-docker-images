FROM alpine:3.12

RUN apk add --no-cache curl bash sudo make

# Install & Initialize Ritchie CLI
RUN curl -fsSL https://commons-repo.ritchiecli.io/install.sh | bash
RUN echo '{"addCommons":false, "sendMetrics":true, "runType":"local"}' | rit init --stdin
RUN rit set formula-runner --runner="local"

# Install Maven & Java 8
RUN apk add --no-cache maven
RUN apk add --no-cache openjdk8
