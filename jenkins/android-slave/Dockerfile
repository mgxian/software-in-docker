FROM openjdk:8-jdk

USER root
RUN dpkg --add-architecture i386 \
    && apt-get update \
    && apt-get install -y sudo git openssh-server rsync unzip \
    && apt-get install -y lib32z1 libbz2-1.0:i386 lib32ncurses5 lib32stdc++6 \
    && rm -rf /var/lib/apt/lists/* \
    && rm -f /var/cache/apt/*.bin

RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd \
    && mkdir -p /var/run/sshd \
    && useradd -m -d /home/jenkins -s /bin/sh jenkins \
    && echo "root:root" | chpasswd \
    && echo "jenkins:jenkins" | chpasswd \
    && echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers \
    && rm -r /etc/ssh/ssh*key \
    && dpkg-reconfigure openssh-server

RUN wget -q https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip \
    && unzip -q sdk-tools-linux-4333796.zip \
    && rm -f sdk-tools-linux-4333796.zip \
    && mkdir /usr/local/android-sdk \
    && mv tools /usr/local/android-sdk \
    && cd /usr/local/android-sdk/tools/bin \
    && ./sdkmanager --update \
    && ./sdkmanager "platforms;android-26" \
    && ./sdkmanager --list

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
