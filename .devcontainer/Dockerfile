
# Created by Devin Headrick 2023, May 4th
# Referencing this: https://github.com/astroesteban/fprime-devcontainer/blob/main/.devcontainer/Dockerfile
#  and Roberts fprime example zip on slack 
FROM debian:bullseye-slim

LABEL Name=cubics_fprime_devcontainer Version=0.0.1

#Setup OS environment
RUN apt-get -y update && \
	apt-get upgrade -y && \
	#This creates a user named 'fprime-dev' and adds them to the sudo group, defines the name 
	#  of the users login shell as /bin/bash , and changes the users password to 'fprime'
	useradd -G "sudo" -s "/bin/bash" -m -U "fprime-dev" && echo "fprime-dev:fprime" | chpasswd && \
	apt-get install --no-install-recommends -y \
	sudo \
	git \
	build-essential \
	docker.io \
	cmake \
	clang-format \
	autoconf \
	automake \
	libtool \
	curl \
	make \
	g++ \
 	unzip \
	wget \
	ca-certificates \
	ssl-cert \
	zlib1g-dev \
	python3 \
	python3-pip \
	#For using fprime tools within a virtual env as recommended in the fprime-community tutorials
	python3-venv  

#TODO set environment variable for $USER to fprime-dev 

#Setup 32bit ARM cross compiling packages in /opt/toolchains dir as per frpime tutorial example:
# https://github.com/fprime-community/fprime-workshop-led-blinker/blob/main/docs/prerequisites.md
RUN mkdir -p /opt/toolchains && \
	chown fprime-dev /opt/toolchains && \
	#Download the provided cross compiling packages for Fprime from ARM
	# Note: Certificates aren't checked rn because it keeps failing (this is not recommended but works...)
	wget --no-check-certificate https://developer.arm.com/-/media/Files/downloads/gnu-a/10.2-2020.11/binrel/gcc-arm-10.2-2020.11-x86_64-arm-none-linux-gnueabihf.tar.xz && \
	#Move file to the /opt/toolchains dir and extract 
	mv gcc-arm-10.2-2020.11-x86_64-arm-none-linux-gnueabihf.tar.xz /opt/toolchains && \
	tar -xvf /opt/toolchains/gcc-arm-10.2-2020.11-x86_64-arm-none-linux-gnueabihf.tar.xz --strip-components=1 -C /opt/toolchains && \
	#Setup build path environment variable 
	export ARM_TOOLS_PATH=/opt/toolchains/bin

#Install pip setup tools
RUN pip3 install -U setuptools setuptools_scm wheel pip

#Install initial bootstrap version of Fprime tools 
RUN pip3 install fprime-tools

CMD [ "/bin/bash", "-l"]