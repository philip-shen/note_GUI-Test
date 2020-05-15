FROM jenkins/jenkins:lts
USER root
RUN apt-get update
RUN apt-get install -y python3-pip
# Install app dependencies
RUN pip3 install --upgrade pip
#RUN pip3 install numpy pandas scipy matplotlib

# install python library
RUN pip3 install atomicwrites \ 
                attrs \ 
                cachetools \ 
                certifi \ 
                chardet \ 
                colorama \ 
                Deprecated \ 
                google-api-core \ 
                google-api-python-client \
                google-auth \ 
                google-auth-httplib2 \ 
                googleapis-common-protos \ 
                httplib2 \ 
                idna \  
                importlib-metadata \ 
                more-itertools \ 
                numpy \ 
                oauth2client \ 
                packaging \ 
                pandas \ 
                pluggy \ 
                protobuf \ 
                py \ 
                pyasn1 \ 
                pyasn1-modules \ 
                PyDrive \ 
                PyGithub \ 
                PyJWT \ 
                pyparsing \ 
                pytest \ 
                python-dateutil \ 
                pytz \ 
                PyYAML \ 
                requests \ 
                rsa \ 
                selenium \ 
                six \ 
                uritemplate \ 
                urllib3 \ 
                wcwidth \ 
                wrapt \ 
                zipp

#install other tools
RUN apt-get install -y \
	vim-common \
	vim \
	curl \
	tftp \
	wget \
	ftp \
	net-tools

# drop back to the regular jenkins user - good practice
USER jenkins