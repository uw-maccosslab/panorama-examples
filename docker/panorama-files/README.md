Simple Docker image which will download and upload files to a [Panorama Server](https://panoramaweb.org/wiki/home/page.view?name=default) such as [PanoramaWeb](https://panoramaweb.org/wiki/home/page.view?name=default).

# Building the docker image 
To build the docker image, simply run 
```
docker image build -t panorama-files:1.0 .
```

# Using the docker image 

## Setup 
To use the docker image, you will need to configure a `.netrc` file to contain the credentials for you Panorama server. A netrc file (.netrc or _netrc) is used to hold credentials necessary to login to your LabKey Server and authorize access to data stored there. The netrc file contains configuration and autologin information when using APIs. It may be used when working with SAS Macros, Transformation Scripts in Java or using the Rlabkey package.

To create the `.netrc` file, follow the instructions on the [Create a netrc file](https://www.labkey.org/Documentation/wiki-page.view?name=netrc) documentation. 

## Download a file 
Below is an example of how you can download a file from your Panorama server 

```
docker run -it -v $HOME/.netrc:/root/.netrc:ro -v $PWD:/data --rm panorama-files:1.0 python /maccoss/download_file.py --url https://panoramaweb.org/_webdav/home/Example Data/Workflow/%40files/Study9S_Site20_v2.sky
```

The file `Study9S_Site20_v2.sky` will be downloaded to the current working directory. 


## Upload a file 
Below is an example for howto upload a file to your Panorama server 
```
docker run -it -v $HOME/.netrc:/root/.netrc:ro -v $PWD:/data --rm panorama-files:1.0 python /maccoss/upload_file.py --url https://panoramaweb.org --folder home/Example Data/Workflow --experiment run_skyline --file-name Site20_STUDY9S_Cond_6ProtMix_QC_01.sky.zip
```

The file `Site20_STUDY9S_Cond_6ProtMix_QC_01.sky.zip` will be uploaded to the folder https://panoramaweb.org/home/Example Data/Workflow/project-begin.view


# Downloading the Docker image 
This docker repo is available on Docker Hub at https://hub.docker.com/repository/docker/proteowizard/panorama-files/general

It can be downloaded by running 

```
docker pull proteowizard/panorama-files:1.0
```
