FROM python:3.7-buster

MAINTAINER "Brian Connolly -- MacCoss Lab"

ENV PATH="/maccoss:${PATH}"

# Create /maccoss directory
RUN mkdir /maccoss \
    && mkdir /data

# Add the required files to the container. These files are added to the /maccoss directory
ADD python-requirements.txt /maccoss
ADD download_file.py /maccoss
ADD upload_file.py /maccoss

# Install required python packages 
RUN pip install -r /maccoss/python-requirements.txt

WORKDIR /data

