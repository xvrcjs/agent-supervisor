FROM cirrusci/flutter:latest

WORKDIR /app
COPY . .

RUN flutter pub get

EXPOSE 8080

CMD ["sh", "-c", "flutter build web --${FLUTTER_BUILD_MODE:-release} && cd build/web && python3 -m http.server 8080"]
