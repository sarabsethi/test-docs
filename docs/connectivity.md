# Connectivity

## Overview

Bugg devices can be connected to the internet to upload audio data (~1.2GB/day [compressed](audio.md#mp3-compression)) and debug logs to a remote server in near real-time. 

When an audio file is recorded by Bugg, it is always first stored to the microSD card. Then, when an upload is initiated in the [recording cycle](audio.md#recording-cycle), Bugg attempts to upload _all_ local audio on the microSD to the remote server.

If a file is uploaded successfully, the local file is deleted. If the upload is unsucessful, the local file is kept, and the upload is retried on the next cycle. 

This safe uploading approach allows Bugg to continue recording continuous datasets through intermittent drops in internet connectivity.

## Mobile internet

With a [nano SIM](hardware.md#side-door), Bugg uploads audio data in near real-time using a mobile internet connection.

Globally, Bugg will connect to 4G LTE networks where possible. In Europe, the Middle East, Africa (EMEA), and Australia there is 3G fallback for areas without 4G coverage. 

The connection is facilitated by the embedded [Sierra Wireless RC7620](https://www.sierrawireless.com/iot-modules/smart-modules/rc7620/){target='blank'} IoT module. 

## Google Cloud Storage

Bugg uploads audio data and debug logs to a Google Cloud Storage (GCS) [bucket](https://cloud.google.com/storage/docs/buckets){target='blank'}. 

### One-off server setup

There are some one-off steps that must be done on the [Google Cloud Console](https://console.cloud.google.com/){target='blank'} to setup a bucket ready for your Bugg devices to upload to. Multiple devices can upload to a single bucket, so this must only be done once per project. 

**1. Create a project**: On the cloud console, create a new project (nav. menu -> IAM and admin -> Create a project)

**2. Create a bucket**: On the cloud console, navigate to buckets (nav. menu -> Cloud Storage -> Buckets) and click the "Create" button at the top of the page. Follow the step-by-step guide.

**3. Create a service account**: To allow the Bugg to access the bucket, set up a service account (nav. menu -> IAM and admin -> Service Accounts) and click the "Create Service Account" button at the top of the page. 

Follow the step-by-step guide, and give the newly created service account the "Storage Object Creator" role for your bucket (see [security](#gcs-security) note).

**4. Export service account key**: On your newly created service account (selected from table on nav. menu -> IAM and admin -> Service Accounts), select the "Keys" tab, then click: Add key -> Create new key. 

On the pop-up, select the "JSON" format and click create. Keep this downloaded file safe as it has details for your Bugg [configuration file](config.md#remote-server-details) and *cannot be downloaded again*. If you do lose it, you will have to generate a new key for the service account. 

### GCS security

Since Buggs are typically deployed unattended for long durations, and the firmware is open source, the service account information that is stored on the device's [configuration file](config.md#remote-server-details) can theoretically be stolen by a malicious actor. 

It is therefore important that you do not give the service account unneccessary permissions (e.g., do not let it read or edit data in the GCS bucket). 

If a service account key is stolen and misused, it can be disabled on the Google Cloud Console. However, this will then mean that all Buggs sharing the same key will not be able to upload data, and will enter offline mode, until the configuration file is updated with a new active GCS key.

It is **strongly advised** that the bucket that the Bugg uploads to is not used for central dataset storage. Instead, we recommend that an appropriate automated service is configured (running on GCS or elsewhere using e.g., the [Python API](https://github.com/googleapis/python-storage){target='blank'}) to regularly move data from the Bugg bucket to a more secure location (another bucket, or another storage provider). In this way, the Bugg bucket is simply used as a temporary dropbox, and long-term data storage is kept more secure.

### GCS costs

[Storage costs](https://cloud.google.com/storage/pricing?gad_source=1&gclid=Cj0KCQjw2PSvBhDjARIsAKc2cgOR_s0XvTiNfSa2BPtpg63mG4hJf2-W6WEGuQKznt6fTzWPRa2A73YaAnzGEALw_wcB&gclsrc=aw.ds){target='blank'} for GCS buckets are relatively inexpensive (~$20/TB/month). However, GCS charges relatively high ["General network usage" fees](https://cloud.google.com/storage/pricing/#price-tables){target='blank'} for downloading data from buckets, or moving data to non-GCS services (~$120/TB). These can become significant if you are moving large volumes of audio data regularly.

Choosing nearline, coldline, or archive storage for your bucket might seem an attractive option due to the lower storage costs. However, do note that these options will incur additional [retrieval fees](https://cloud.google.com/storage/pricing/#retrieval-pricing){target='blank'} (up to $50/TB) when you want to access/download/move your data.  

There are two recommended approaches to manage Bugg GCS costs:

* As soon as audio is uploaded to the Bugg bucket, copy it to a local server and delete it from GCS
* Use Google Cloud for your full analysis pipeline and only download results or small amounts of audio as needed 

## Offline mode

If online operation is not desired or possible, the device can be run in offline mode without a nano SIM, and audio can be retrieved manually from the microSD card at the end of deployments. 

Audio from offline Buggs can be uploaded through the same pipeline that online devices use, i.e., to the same GCS bucket in the same file formats, using the `bugg-offline-uploader` [command line tool](https://github.com/bugg-resources/bugg-offline-uploader){target='blank'}.

Please note, the offline uploader tool was built for devices using the Bugg web app (not openly available), but should work for other GCS buckets.
