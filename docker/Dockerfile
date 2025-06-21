FROM alpine:3.19.1 as builder
ADD "https://api.github.com/repos/ReversecLabs/drozer/commits?per_page=1" latest_commit
RUN apk add --no-cache \
		openjdk11-jdk \
		python3-dev \
		git \
		py3-pip \
		py3-openssl \
		py3-twisted \
		py3-yaml \
		py3-distro \
		py3-service_identity \
		protobuf-dev \
		bash \
		gcc \
		py3-wheel && \
	wget https://github.com/ReversecLabs/drozer/archive/refs/heads/develop.zip -O drozer.zip && \
	unzip drozer.zip && \
	mv drozer*/ drozer/ && \
	cd drozer && \
	pip install --no-cache-dir --break-system-packages \
		-r requirements.txt && \
	pip install --no-cache-dir --break-system-packages\
	    build && \
	python -m build

FROM alpine:3.19.1
ADD "https://api.github.com/repos/ReversecLabs/drozer/commits?per_page=1" latest_commit
RUN --mount=target=/builder,from=builder,source=/drozer apk add --no-cache \
		openjdk11-jre-headless \
		python3 \
		bash \
		musl-dev \
		gcc \
		python3-dev \
		libffi-dev \
		vim \
		py3-openssl \
		py3-twisted \
		py3-distro \
		py3-service_identity \
		py3-yaml \
		py3-pip && \
	pip install /builder/dist/drozer*.whl --no-cache-dir --break-system-packages && \
	apk del --purge --no-cache \
		py3-pip \
		gcc \
		bash \
		musl-dev \
		python3-dev \
		libffi-dev
ENTRYPOINT ["drozer"]
