ARG BASE_IMAGE=m1k1o/neko:base
FROM $BASE_IMAGE

ARG SRC_URL="https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US"

# Installer Firefox
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends openbox xz-utils bzip2 libgtk-3-0 wget; \
    wget -O /tmp/firefox-setup.tar.bz2 "${SRC_URL}"; \
    mkdir -p /usr/lib/firefox; \
    tar -xjf /tmp/firefox-setup.tar.bz2 -C /usr/lib; \
    rm -f /tmp/firefox-setup.tar.bz2; \
    ln -s /usr/lib/firefox/firefox /usr/bin/firefox; \
    \
    # Créer un utilisateur alternatif (pour éviter conflits)
    useradd -m neko2; \
    mkdir -p /home/neko2/.mozilla/firefox/profile.default/extensions; \
    chown -R neko2:neko2 /home/neko2; \
    \
    apt-get --purge autoremove -y xz-utils bzip2; \
    apt-get clean -y; \
    rm -rf /var/lib/apt/lists/* /var/cache/apt/*

# Copier les fichiers avec le bon user
COPY supervisord.conf /etc/neko/supervisord/firefox.conf
COPY neko.js /usr/lib/firefox/mozilla.cfg
COPY autoconfig.js /usr/lib/firefox/defaults/pref/autoconfig.js
COPY policies.json /usr/lib/firefox/distribution/policies.json
COPY --chown=neko2:neko2 profiles.ini /home/neko2/.mozilla/firefox/profiles.ini
COPY openbox.xml /etc/neko/openbox.xml

