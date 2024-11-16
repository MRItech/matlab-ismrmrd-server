# --------------------------------------------------------------------------------------------------
#            Instructions for making a Docker image of a standalone MATLAB application on version R2020a
# --------------------------------------------------------------------------------------------------
# 	This instructions assumes that you have a MATLAB license for version R2020a
#
# 1.	Prepare Matlab standalone application in a Linux version of MALTAB R2020a:
#  This can be done using the "mcc" command, e.g: 
#  mcc -o fire_matlab_ismrmrd_server -W main:fire_matlab_ismrmrd_server -T link:exe -d /home/nashwan/fire_matlab_server/fire_matlab_server/for_testing -v /home/nashwan/fire-matlab-ismrmrd-server/fire_matlab_ismrmrd_server.m -a /home/nashwan/fire-matlab-ismrmrd-server/additional_files 
# 
# OR using the "application compiler" app inside MATLAB, as follows:
# 	a.	Run Matlab in Linux
#	b.	Open application compiler
#	c.	Select “fire_matlab_server.m” as the main file
#	d.	Add any additional required files under “files required for your application to run”
#	e.	Select whether Matlab runtime (MCR) should be included in the package or downloaded from the web.
#	f.	Press “package”
#
#
# 2.	Download Matlab Compiled Runtime (MCR) for the version R2020a, if you haven’t already: 
# 	a.	from this link: https://ssd.mathworks.com/supportfiles/downloads/R2020a/Release/8/deployment_files/installer/complete/glnxa64/MATLAB_Runtime_R2020a_Update_8_glnxa64.zip
# 	b.	Note the location where the MRC is installed, e.g. /usr/local/MATLAB/MATLAB_Runtime/v98
#
#
# 3.	Prepare all the required files in one folder:
#	a.	Make folder to put all required files together in one place
#		mkdir /home/Nashwan/test_docker_from_MATLAB2020a
#
#	b.	Copy the compiled app to this folder, only copy files found inside the folder “for_redistribution_files_only”:
# 		cp -a /home/nashwan/fire_matlab_server/fire_matlab_ismrmrd_server/for_redistribution_files_only/. /home/nashwan/test_docker_from_MATLAB2020a/applicationFilesForMATLABCompiler/
#
#	c. 	copy Matlab Compiled Runtime (MCR) to this folder
#		cp -r /usr/local/MATLAB/MATLAB_Runtime/v98 /home/nashwan/test_docker_from_MATLAB2020a/
#
#	d.	Put THIS Dockerfile in the same folder.
#
#	e.	Finally, from inside the folder, run the following to make the Docker image:
#		docker build --tag 'fire-matlab-server20a' .
#
# --------------------------------------------------------------------------------------------------
# 			The Docker image should appear in your Docker manager. To test it, use this command:
#  
# docker run --gpus all --rm -v /path_of_local_folder:/tmp/share -p 9002:9002 fire-matlab-server20a
#
#
#
#
#
# # --------------------------------------------------------------------------------------------------
# Copyright 2020-2022 The MathWorks, Inc.
# base image

FROM ubuntu:20.04

MAINTAINER MathWorks

ENV TZ="Etc/UTC"

RUN apt-get update && apt-get install --no-install-recommends -y \
ca-certificates \
libasound2 \
libatk-bridge2.0-0 \
libatk1.0-0 \
libatspi2.0-0 \
libc6 \
libcairo-gobject2 \
libcairo2 \
libcap2 \
libcrypt1 \
libcups2 \
libdbus-1-3 \
libdrm2 \
libfontconfig1 \
libgbm1 \
libgdk-pixbuf2.0-0 \
libgl1 \
libglib2.0-0 \
libgstreamer-plugins-base1.0-0 \
libgstreamer1.0-0 \
libgtk-3-0 \
libnspr4 \
libnss3 \
libodbc1 \
libpam0g \
libpango-1.0-0 \
libpangocairo-1.0-0 \
libpangoft2-1.0-0 \
libpython3.9 \
libsm6 \
libsndfile1 \
libssl1.1 \
libuuid1 \
libx11-6 \
libx11-xcb1 \
libxcb-dri3-0 \
libxcb1 \
libxcomposite1 \
libxcursor1 \
libxdamage1 \
libxext6 \
libxfixes3 \
libxft2 \
libxi6 \
libxinerama1 \
libxrandr2 \
libxrender1 \
libxt6 \
libxtst6 \
libxxf86vm1 \
locales \
locales-all \
make \
net-tools \
procps \
sudo \
unzip \
zlib1g \
&& apt-get clean && apt-get -y autoremove && rm -rf /var/lib/apt/lists/*

RUN [ -d /usr/share/X11/xkb ] || mkdir -p /usr/share/X11/xkb


RUN apt-get update -y && apt-get install --no-install-recommends -y libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-libav \
gstreamer1.0-plugins-base \
gstreamer1.0-plugins-good \
&& apt-get clean && apt-get -y autoremove && rm -rf /var/lib/apt/lists/*

# copy runtime 
COPY ./v98 /opt/matlabruntime/v98
ENV LD_LIBRARY_PATH /opt/matlabruntime/v98/runtime/glnxa64:/opt/matlabruntime/v98/bin/glnxa64:/opt/matlabruntime/v98/sys/os/glnxa64:/opt/matlabruntime/v98/sys/opengl/lib/glnxa64:/opt/matlabruntime/v98/extern/bin/glnxa64

# copy required files for the compiled application 
COPY ./applicationFilesForMATLABCompiler /usr/bin/mlrtapp
RUN chmod -R a+rX /usr/bin/mlrtapp/*
RUN useradd -ms /bin/bash appuser
USER appuser
ENTRYPOINT ["/usr/bin/mlrtapp/fire_matlab_ismrmrd_server"]