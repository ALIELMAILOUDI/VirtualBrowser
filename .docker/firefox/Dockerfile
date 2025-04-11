ARG BASE_IMAGE=m1k1o/neko:base
FROM $BASE_IMAGE

ARG SRC_URL="https://ftp.mozilla.org/pub/firefox/releases/121.0/linux-x86_64/en-US/firefox-121.0.tar.bz2"

RUN apt-get update
RUN apt-get install -y --no-install-recommends openbox xz-utils bzip2 libgtk-3-0 wget file
RUN wget -O /tmp/firefox-setup.tar.bz2 "${SRC_URL}"
RUN file /tmp/firefox-setup.tar.bz2
RUN mkdir -p /usr/lib/firefox
RUN tar -xjf /tmp/firefox-setup.tar.bz2 -C /usr/lib
RUN rm -f /tmp/firefox-setup.tar.bz2
RUN ln -s /usr/lib/firefox/firefox /usr/bin/firefox
RUN mkdir -p /root/.mozilla/firefox/profile.default/extensions
RUN apt-get --purge autoremove -y xz-utils bzip2
RUN apt-get clean -y
RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/*

COPY supervisord.conf /etc/neko/supervisord/firefox.conf
COPY neko.js /usr/lib/firefox/mozilla.cfg
COPY autoconfig.js /usr/lib/firefox/defaults/pref/autoconfig.js
COPY policies.json /usr/lib/firefox/distribution/policies.json
COPY profiles.ini /root/.mozilla/firefox/profiles.ini
COPY openbox.xml /etc/neko/openbox.xml

