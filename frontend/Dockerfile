FROM ubuntu:22.04

# 1. Install system dependencies
RUN apt-get update && apt-get install -y git curl unzip xz-utils zip wget libarchive-tools openjdk-11-jdk libglu1-mesa && rm -rf /var/lib/apt/lists/*

# 2. Configure Android SDK
ENV ANDROID_SDK_ROOT="/opt/android-sdk"
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools && \
    wget -qO- https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip | bsdtar -xvf- -C $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    chmod -R a+rx $ANDROID_SDK_ROOT/cmdline-tools/latest/bin

# 3. Install Android tools and accept licenses
ENV PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH"
RUN useradd -ms /bin/bash flutteruser && \
    yes | sdkmanager --sdk_root=$ANDROID_SDK_ROOT --licenses && \
    sdkmanager --sdk_root=$ANDROID_SDK_ROOT "platform-tools" "platforms;android-33" "build-tools;33.0.0" "build-tools;30.0.3" && \
    chown -R flutteruser:flutteruser $ANDROID_SDK_ROOT

# 4. Clone Flutter
RUN git clone --branch 3.10.5 https://github.com/flutter/flutter.git /opt/flutter && \
    chown -R flutteruser:flutteruser /opt/flutter && \
    git config --global --add safe.directory /opt/flutter

# 5. Configure Flutter PATH
ENV PATH="/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:$PATH"

# 6. Copy project and build APK
WORKDIR /app
COPY . /app
RUN chown -R flutteruser:flutteruser /app
USER flutteruser
RUN flutter create . --template app
RUN flutter pub get
RUN flutter build apk --release

# 8. Expose port
EXPOSE 8080
