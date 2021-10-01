# for Apple M1
FROM --platform=linux/x86_64 mysql:8.0

# set locale
RUN apt-get update && \
    apt-get install -y vim locales && \
    echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen ja_JP.UTF-8
ENV LC_ALL ja_JP.UTF-8
