FROM buildpack-deps:jessie-curl

# Install dependencies
RUN apt-get update && apt-get install -y \
	fuse \
	golang-go\
	libappindicator1 \
	gconf-service \
	gconf2-common \
	libdbus-glib-1-2 \ 
	libgconf-2-4 psmisc \
	git \
	--no-install-recommends

# Get and verify Keybase.io's code signing key
RUN curl https://keybase.io/docs/server_security/code_signing_key.asc | \
	gpg --import \
	&& gpg --fingerprint 222B85B0F90BE2D24CFEB93F47484E50656D16C7

# Get, verify and install client package
RUN curl -O https://prerelease.keybase.io/keybase_amd64.deb.sig \
	&& curl -O https://prerelease.keybase.io/keybase_amd64.deb \
	&& gpg --verify keybase_amd64.deb.sig keybase_amd64.deb \
	&& dpkg -i keybase_amd64.deb \
	&& apt-get install -f

#Create group, user
RUN groupadd -g 1000 keybase \
	&& useradd --create-home -g keybase -u 1000 keybase 

# Cleanup
RUN rm -r /var/lib/apt/lists/* \
	&& rm keybase_amd64.deb*

USER keybase
RUN mkdir /home/keybase/go
ENV GOPATH=$HOME/go
WORKDIR /home/keybase
CMD ["bash"]

RUN run_keybase