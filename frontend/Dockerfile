FROM ubuntu:22.04

# 1. Install system dependencies
RUN apt-get update && apt-get install -y git curl unzip xz-utils zip wget libarchive-tools openjdk-11-jdk libglu1-mesa && rm -rf /var/lib/apt/lists/*

# 2. Configure Android SDK
ENV ANDROID_SDK_ROOT="/opt/android-sdk"
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    wget -qO- https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip | bsdtar -xvf- -C $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    chmod -R a+rx $ANDROID_SDK_ROOT/cmdline-tools/latest/bin && \
    yes | sdkmanager --sdk_root=$ANDROID_SDK_ROOT --licenses && \
    sdkmanager --sdk_root=$ANDROID_SDK_ROOT "platform-tools" "platforms;android-33" "build-tools;33.0.0" "build-tools;30.0.3"

# 3. Install Flutter and create user
RUN git clone --branch 3.10.5 https://github.com/flutter/flutter.git /opt/flutter && \
    useradd -ms /bin/bash flutteruser && \
    chown -R flutteruser:flutteruser /opt/flutter && \
    git config --global --add safe.directory /opt/flutter
ENV PATH="/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:$PATH"

# 4. Generate temporary project and merge code
WORKDIR /
RUN flutter create temp_app --template app
COPY lib/ temp_app/lib/
COPY assets/ temp_app/assets/
COPY pubspec.yaml temp_app/pubspec.yaml
COPY android/ temp_app/android/
COPY ios/ temp_app/ios/

# 5. Adjust permissions and build
RUN chown -R flutteruser:flutteruser $ANDROID_SDK_ROOT /opt/flutter temp_app
USER flutteruser
WORKDIR /temp_app
RUN flutter pub get && flutter build apk --release

# 6. Expose port
EXPOSE 8080
