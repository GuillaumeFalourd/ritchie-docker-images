FROM alpine:3.12

RUN apk add --no-cache curl bash sudo

RUN curl -fsSL https://commons-repo.ritchiecli.io/install.sh | bash
RUN echo '{"addCommons":false, "sendMetrics":true, "runType":"local"}' | rit init --stdin

ENV PATH /usr/local/go/bin:$PATH

ENV GOLANG_VERSION 1.15.8

RUN set -eux; \
	apk add --no-cache \
		bash \
		gcc \
		musl-dev \
        ca-certificates \
        make \
        go \
	; \
	export \
		GOROOT_BOOTSTRAP="$(go env GOROOT)" \
		GOOS="$(go env GOOS)" \
		GOARCH="$(go env GOARCH)" \
		GOHOSTOS="$(go env GOHOSTOS)" \
		GOHOSTARCH="$(go env GOHOSTARCH)" \
	; \
	apkArch="$(apk --print-arch)"; \
	case "$apkArch" in \
		armhf) export GOARM='6' ;; \
		armv7) export GOARM='7' ;; \
		x86) export GO386='387' ;; \
	esac; \
	url='https://storage.googleapis.com/golang/go1.15.8.src.tar.gz'; \
	sha256='540c0ab7781084d124991321ed1458e479982de94454a98afab6acadf38497c2'; \
	wget -O go.tgz.asc "$url.asc"; \
	wget -O go.tgz "$url"; \
	echo "$sha256 *go.tgz" | sha256sum -c -; \
	tar -C /usr/local -xzf go.tgz; \
	rm go.tgz; \
	goEnv="$(go env | sed -rn -e '/^GO(OS|ARCH|ARM|386)=/s//export \0/p')"; \
	eval "$goEnv"; \
	[ -n "$GOOS" ]; \
	[ -n "$GOARCH" ]; \
	( \
		cd /usr/local/go/src; \
		./make.bash; \
	); \
	\
	go install std; \
	\
	rm -rf \
		/usr/local/go/pkg/*/cmd \
		/usr/local/go/pkg/bootstrap \
		/usr/local/go/pkg/obj \
		/usr/local/go/pkg/tool/*/api \
		/usr/local/go/pkg/tool/*/go_bootstrap \
		/usr/local/go/src/cmd/dist/dist \
	; \
	\
	go version

ENV GOPATH /go
ENV PATH $GOPATH/bin:$PATH
RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

