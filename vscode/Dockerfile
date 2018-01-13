# Visual Studio Code in a container
#	NOTE: Needs the redering device (yeah... idk)
#
# docker run -d \
#    -v /tmp/.X11-unix:/tmp/.X11-unix \
#    -v $HOME:/home/user \
#    -e DISPLAY=unix$DISPLAY \
#    --device /dev/dri \
#    --name vscode \
#    jess/vscode

# Now on one line for your copy-pasters:
# docker run -d -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME:/home/computer -e DISPLAY=unix$DISPLAY --device /dev/dri --name vscode jess/vscode

FROM debian:buster
LABEL maintainer "Jessie Frazelle <jess@linux.com>"

# Tell debconf to run in non-interactive mode
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
	apt-transport-https \
	ca-certificates \
	curl \
	gnupg \
	--no-install-recommends

# Add the vscode debian repo
RUN curl -sSL https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | apt-key add -
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list

RUN apt-get update && apt-get -y install \
	code \
	libasound2 \
	libatk1.0-0 \
	libcairo2 \
	libcups2 \
	libexpat1 \
	libfontconfig1 \
	libfreetype6 \
	libgtk2.0-0 \
	libpango-1.0-0 \
	libx11-xcb1 \
	libxcomposite1 \
	libxcursor1 \
	libxdamage1 \
	libxext6 \
	libxfixes3 \
	libxi6 \
	libxrandr2 \
	libxrender1 \
	libxss1 \
	libxtst6 \
	git \
	python3.6 \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

ENV HOME /home/computer
RUN useradd --create-home --home-dir $HOME computer \
	&& chown -R computer:computer $HOME

RUN curl -sSL http://mirrors.kernel.org/ubuntu/pool/main/f/freetype/libfreetype6_2.8-0.2ubuntu2_amd64.deb -o /home/computer/freetype.deb
RUN dpkg -i /home/computer/freetype.deb

RUN /usr/bin/code --user-data-dir=/home/computer --install-extension ms-python.python
RUN /usr/bin/code --user-data-dir=/home/computer --install-extension donjayamanne.git-extension-pack


COPY start.sh /usr/local/bin/start.sh

WORKDIR $HOME

CMD [ "start.sh" ]
