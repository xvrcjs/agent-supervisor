FROM ubuntu:22.04

RUN apt-get update \
    && apt-get install -y git curl unzip xz-utils zip openjdk-11-jdk libglu1-mesa \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --branch 3.10.5 https://github.com/flutter/flutter.git /opt/flutter

RUN useradd -ms /bin/bash flutteruser

RUN chown -R flutteruser:flutteruser /opt/flutter \
    && git config --global --add safe.directory /opt/flutter

ENV PATH="/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:${PATH}"

COPY . /app
WORKDIR /app
RUN flutter create . --template app

USER flutteruser
RUN flutter pub get \
    && flutter build apk --release

EXPOSE 8080
