# Learn Fast — Agent SDKs workbench (smolagents browser agent)
#
# An EMPTY environment: the libraries this track needs, a headless Firefox, and none of the course notebooks.
# You write your own notebooks in work/ — that folder is what you submit for the drill.
FROM quay.io/jupyter/minimal-notebook:python-3.12

# NOTE: the user is renamed jovyan -> learnfast at RUN time (see docker-compose.yml), never here:
# upstream's start.sh calls `id -u jovyan` under `set -e`, so renaming in the image makes the
# container exit 1 on startup with no error message.

# A real browser for the browser-agent drill. Ubuntu's apt "chromium" and "firefox" are snap stubs that do
# not run in a container, and Chrome for Testing has no Linux arm64 build. Mozilla's own apt repository
# ships Firefox for both amd64 and arm64, and helium supports it.
USER root
RUN install -d -m 0755 /etc/apt/keyrings \
 && wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O /etc/apt/keyrings/packages.mozilla.org.asc \
 && echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" > /etc/apt/sources.list.d/mozilla.list \
 && printf 'Package: *\nPin: origin packages.mozilla.org\nPin-Priority: 1000\n' > /etc/apt/preferences.d/mozilla \
 && apt-get update && apt-get install -y --no-install-recommends firefox fonts-dejavu-core \
 && rm -rf /var/lib/apt/lists/*

# Selenium Manager's cache; geckodriver is baked in below.
RUN mkdir -p /opt/selenium && chown ${NB_UID}:${NB_GID} /opt/selenium

USER ${NB_UID}
COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Bake geckodriver (the program Selenium uses to drive Firefox) into the image, so the first run needs no
# download. Selenium Manager fetches it into SE_CACHE_PATH the first time Firefox is started.
ENV SE_CACHE_PATH=/opt/selenium
RUN python -c "from selenium import webdriver; from selenium.webdriver.firefox.options import Options; \
o=Options(); o.add_argument('-headless'); d=webdriver.Firefox(options=o); d.quit()"
