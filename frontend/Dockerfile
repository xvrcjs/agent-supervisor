FROM cirrusci/flutter:stable

# Create non-root user for running Flutter commands
RUN useradd -ms /bin/bash flutteruser

WORKDIR /app
COPY --chown=flutteruser:flutteruser . .

RUN chown -R flutteruser:flutteruser /sdks/flutter && \
    git config --global --add safe.directory /sdks/flutter

USER flutteruser

RUN flutter pub get

EXPOSE 8080

CMD ["sh", "-c", "flutter build web --${FLUTTER_BUILD_MODE:-release} && cd build/web && python3 -m http.server 8080"]
