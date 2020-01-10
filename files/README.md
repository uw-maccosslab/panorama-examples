In this folder you will find code examples for working with files stored on a Panorama Server. 


## Getting Started 
For these examples we will work with files in the [Example Data](https://panoramaweb.org/filecontent/home/Example%20Data/begin.view?) folder on PanoramaWeb. 


### Setup .netrc 

A netrc file (.netrc or _netrc) is used to hold credentials necessary to login to your LabKey Server and authorize access to data stored there. The netrc file contains configuration and autologin information when using APIs. It may be used when working with SAS Macros, Transformation Scripts in Java or using the Rlabkey package.

To create the `.netrc` file, follow the instructions on the [Create a netrc file](https://www.labkey.org/Documentation/wiki-page.view?name=netrc&_docid=wiki%3A32d70bfc-ed56-1034-b734-fe851e088836) documentation. 


## Download a file from the folder 

To get started we will download a file from the PanoramaWeb server. In this example we will download the file a single file `Study9p1_site_56A_Merged.sky.log` from the URL https://panoramaweb.org/_webdav/home/Example%20Data/%40files/api_examples/Study9p1_site_56A_Merged.sky.log

### Python
The example code below assumes you are using Python 3

```python
import requests

url = "https://panoramaweb.org/_webdav/home/Example%20Data/%40files/api_examples/Study9p1_site_56A_Merged.sky.log"
local_filename = url.split('/')[-1]

with open(local_filename, 'wb') as f:
    resp = requests.get(url, stream=True)
    for chunk in resp.iter_content(chunk_size=1024):
        if chunk:  # filter out keep-alive new chunks
            f.write(chunk)

print("{} has been downloaded.".format(local_filename))
```

See [download_file.py](download_file.py) for a working example of how to download a file from a Panorama Server. 


## Uploading a new file to the folder 

Now lets try and upload a file to panoramaweb. In this example we will upload the file a single file `Study9p1_site_56A_Merged.sky.log` from the PanoramaWeb folder `home/Example Data`. The file will be uploaded into the `api_examples` directory in the [Fileroot](https://panoramaweb.org/filecontent/home/Example%20Data/begin.view?). 

### Python

When uploading a file, you will need to make two requests. This is because PanoramaWeb has enabled [Cross-Site Request Forgery (CSRF) Protection](https://www.labkey.org/Documentation/wiki-page.view?name=csrfProtection). The first request will authenticate to the server and the 2nd request will upload the file. 

```python
import requests

# Server Variables
server = "https://panoramaweb.org"  # URI of the Panorama server
folder_path = "home/Example%20Data"  # Folder where the file will be uploaded

# File to be uploaded
file_to_be_uploaded = "/Users/bconn/tmp/maccoss/s3testing/skylinefiles/MethodEditTutorial-test1_2018-03-01_15-06-43.sky.zip"

# Authenticate to server
csrf_url = "{}/login/{}/whoami.api".format(
    server,
    folder_path
)
csrf_response = requests.get(csrf_url)
csrf_response_json = csrf_response.json()
csrf_response.close()

csrf_header = csrf_response_json['CSRF']
csrf_cookies = requests.utils.dict_from_cookiejar(csrf_response.cookies)

# Create URL which will be used to upload the file
url = "{}/_webdav/{}/%40files/?Accept=application/json&overwrite=T&X-LABKEY-CSRF={}".format(
    server,
    folder_path,
    csrf_header
)

# Create the dictionary containing the file name and other file attributes
files = {
    'file': (
        'MethodEditTutorial-test1_2018-03-01_15-06-43.sky.zip', open(file_to_be_uploaded, 'rb'),
        'application/octet-stream',
        {'Expires': '0'}
    )
}

# Upload the file
upload_response = requests.post(url, files=files, cookies=csrf_cookies)

# Check if upload was successful
#   HTTP response code of 207 indicates success.
#   All other response codes indicate failure

if upload_response.status_code == 207:
    print("{} has been uploaded.".format(file_to_be_uploaded))
else:
    upload_text = resp.text.split('{')[1].split('}')[0].split(',')
    print("Upload of {} has failed".format(file_to_be_uploaded))
    print(
        "HTTP reponse from the server is {}. "
        "Status message from the server is {}".format(
            resp.status_code,
            upload_text
        )
    )

```

See [upload_file.py](upload_file.py) for a working example of how to upload a file to a Panorama Server. 


## Listing files in the folder 

The example code will be added here in the near future.

