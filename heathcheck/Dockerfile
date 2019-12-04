FROM ubuntu:16.04

# Install dependencies
RUN apt-get update && apt-get install -y \
	git \
	curl \
	wget \
	ca-certificates \
	--no-install-recommends

# Get health-checker
WORKDIR /bin
RUN curl -s https://api.github.com/repos/gruntwork-io/health-checker/releases/latest \
  | grep browser_download_url \
  | grep linux_amd64 \
  | cut -d '"' -f 4 \
  | wget -qi -

RUN mv health-checker_linux_amd64 health-checker
RUN chmod +x /bin/health-checker

#Create group, user
RUN groupadd -g 1000 heathcheck \
	&& useradd --create-home -g heathcheck -u 1000 heathcheck 

# Cleanup
RUN rm -r /var/lib/apt/lists/*

USER heathcheck
WORKDIR /home/heathcheck
