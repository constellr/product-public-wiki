# Google Cloud Storage via gsutil tool
For the full documentation on gsutil take a look at [google cloud documentation](https://cloud.google.com/storage/docs/gsutil).

gsutil is a Python application that lets you access Cloud Storage from the command line. One of its applications is to download objects saved in a bucket. 

First you will need to install gsutil. Follow [this link](https://cloud.google.com/storage/docs/gsutil_install#windows) and follow the intructions based on your operating system.

Once it is installed you can use the Google command line to download the needed data from a bucket. For that use the `gsutil cp` command:

```python
gsutil cp gs://BUCKET_NAME/OBJECT_NAME SAVE_TO_LOCATION
```
Where:

- `BUCKET_NAME` is the name of the bucket containing the object you are downloading. For example, my-bucket.
- `OBJECT_NAME` is the name of object you are downloading. For example, pets/dog.png.
- `SAVE_TO_LOCATION` is the local path where you are saving your object. For example, Desktop/Images.

In case you wish to download multiple files or whole folders, you will need to add the `-r` flag to the command:

```python
gsutil cp -r gs://BUCKET_NAME/OBJECT_NAME SAVE_TO_LOCATION
```

To list, for example, the tif files in the bucket:
```python
gsutil ls gs://BUCKET_NAME/*.tif
```

# Google Cloud Storage via REST API

You can also access Google Cloud Storage via REST API.
To get a full understanding of how to access Google Cloud Storage data via REST API, I highly suggest taking a look at [google cloud documentation](https://cloud.google.com/storage/docs/downloading-objects#rest-download-object) on this but to make things simple, here is a summarised version of it.

### List files from google cloud storage

1. You will need an authentification token that we’ll call `OAUTH2_TOKEN`. To get it, run the `gcloud auth print-access-token` (into the google cloud console or wherever you have gcloud installed)
2. Use `[cURL](https://curl.haxx.se/)` to call the [JSON API](https://cloud.google.com/storage/docs/json_api) with a `[GET` Object](https://cloud.google.com/storage/docs/json_api/v1/objects/get) request:

```python
curl -X GET \
  -H "Authorization: Bearer OAUTH2_TOKEN" \
  -o "SAVE_TO_LOCATION" \
  "https://storage.googleapis.com/storage/v1/b/BUCKET_NAME/o"
```

Where:

- **`OAUTH2_TOKEN`** is the access token you generated in Step 1.
- **`SAVE_TO_LOCATION`** is the local path where you want to save your object. For example `/Users/joyvan/listed_files.json`.
- **`BUCKET_NAME`** is the name of the bucket where you want to check the content. For example `my-bucket`.

### Get a file from google cloud storage

In a similar way, if you want to download a specific file, use the following command:

```python
curl -X GET \
  -H "Authorization: Bearer OAUTH2_TOKEN" \
  -o "SAVE_TO_LOCATION" \
  "https://storage.googleapis.com/storage/v1/b/BUCKET_NAME/o/OBJECT_NAME?alt=media"
```

Where:

- **`OAUTH2_TOKEN`** is the access token.
- **`SAVE_TO_LOCATION`** is the local path where you want to save your object. For example `/Users/joyvan/LST_IMAGE.tif`.
- **`BUCKET_NAME`** is the name of the bucket containing the object you are downloading. For example `my-bucket`.
- **`OBJECT_NAME`** is the URL-encoded name of the object you are downloading for example `LST_IMAGE.tif`. Note, URL-encoded means`/`characters needs to be replaced by `%2`.
- **`SAVE_TO_LOCATION`** is the local path where you want to save your object. For example, `/Users/joyvan/image.tif`.
