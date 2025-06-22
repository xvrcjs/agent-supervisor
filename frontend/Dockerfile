FROM cirrusci/flutter:latest

RUN git clone --branch 3.10.5 https://github.com/flutter/flutter.git /opt/flutter && \
    useradd -ms /bin/bash flutteruser && \
    chown -R flutteruser:flutteruser /opt/flutter && \
    git config --global --add safe.directory /opt/flutter

ENV PATH="/opt/flutter/bin:${PATH}"

COPY --chown=flutteruser:flutteruser . /app
WORKDIR /app
USER flutteruser

RUN flutter pub get && \
    flutter build apk --release
