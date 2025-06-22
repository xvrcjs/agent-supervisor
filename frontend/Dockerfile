FROM cirrusci/flutter:stable

# Create non-root user for running Flutter commands
RUN useradd -ms /bin/bash flutteruser

WORKDIR /app
COPY --chown=flutteruser:flutteruser . .

USER flutteruser

RUN flutter pub get

EXPOSE 8080

CMD ["sh", "-c", "flutter build web --${FLUTTER_BUILD_MODE:-release} && cd build/web && python3 -m http.server 8080"]
