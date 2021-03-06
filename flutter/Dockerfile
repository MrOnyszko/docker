FROM ubuntu:bionic

ARG flutter_version

ENV ANDROID_HOME=/opt/android-sdk-linux \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    FLUTTER_HOME=/root/sdks/flutter \
    FLUTTER_ROOT=$FLUTTER_HOME \
    FLUTTER_VERSION=$flutter_version

ENV PATH ${PATH}:${FLUTTER_HOME}/bin:${FLUTTER_HOME}/bin/cache/dart-sdk/bin

ENV ANDROID_SDK_ROOT=$ANDROID_HOME \
    PATH=${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools

# comes from https://developer.android.com/studio/
ENV ANDROID_SDK_TOOLS_VERSION 4333796

RUN cd /opt \
    && apt-get update \
    && apt-get install -y openjdk-8-jdk \
    && apt-get install -y sudo wget zip unzip git openssh-client curl software-properties-common build-essential ruby-full ruby-bundler lib32stdc++6 libstdc++6 libpulse0 libglu1-mesa locales lcov --no-install-recommends \
    # for x86 emulators
    && apt-get install -y libxtst6 libnss3-dev libnspr4 libxss1 libasound2 libatk-bridge2.0-0 libgtk-3-0 libgdk-pixbuf2.0-0 \
    && rm -rf /var/lib/apt/lists/* \
    && sudo sh -c 'echo "en_US.UTF-8 UTF-8" > /etc/locale.gen' \
    && sudo locale-gen \
    && sudo update-locale LANG=en_US.UTF-8 \
    && wget -q https://dl.google.com/android/repository/sdk-tools-linux-$ANDROID_SDK_TOOLS_VERSION.zip -O android-sdk-tools.zip \
    && unzip -q android-sdk-tools.zip -d ${ANDROID_HOME} \
    && rm android-sdk-tools.zip \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && yes | sdkmanager --licenses \
    && wget -O /usr/bin/android-wait-for-emulator https://raw.githubusercontent.com/travis-ci/travis-cookbooks/master/community-cookbooks/android-sdk/files/default/android-wait-for-emulator \
    && chmod +x /usr/bin/android-wait-for-emulator \
    && touch /root/.android/repositories.cfg \
    && sdkmanager tools \
    && sdkmanager platform-tools \
    && sdkmanager emulator

RUN curl -sL https://firebase.tools | bash

# Install Flutter
RUN git clone --branch ${FLUTTER_VERSION} https://github.com/flutter/flutter.git ${FLUTTER_HOME} && \
    flutter doctor