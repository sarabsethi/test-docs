# Configuration file

## Overview

The Bugg device loads its configuration from a `config.json` file which must be placed on the microSD card [inserted into the device](hardware.md#side-door). The name of the configuration file must be exact or the device will not be able to locate it.

A single `config.json` file can be used for multiple Buggs under the same project (assuming SIM cards are on similar plans). 

There is no need to change any parameters within the configuration file to individually identify devices. Devices are uniquely identified by a unique hardware ID baked into the CPU of each device, which is either printed on the enclosure or supplied upon delivery. 

### Structure of `config.json`

There are four main components of the `config.json` file:

1. General information
1. Audio recording parameters
1. Mobile internet settings
1. Remote server details

Configuration files should be based on the below template:

```
{
   "device": {
      "gcs_bucket_name": "bugg-audio-dropbox",
      "project_id": "taiwan",
      "config_id": "a35c532"
   }, 

   "sensor": {
      "record_length": 300,
      "capture_delay": 0,
      "record_freq": 44100,
      "compress_data": true,
      "sensor_type": "I2SMic"
   },
   
   "mobile_network": {
      "hostname": "internet",
      "password": "blank",
      "username": "blank"
   },

   "type": "service_account",
   "project_id": "XXXX",
   "private_key_id": "XXXX",
   "private_key": "-----BEGIN PRIVATE KEY-----\nXXXX\n-----END PRIVATE KEY-----\n",
   "client_email": "XXXX",
   "client_id": "XXXX",
   "auth_uri": "XXXX",
   "token_uri": "XXXX",
   "auth_provider_x509_cert_url": "XXXX",
   "client_x509_cert_url": "XXXX"
}
```
## Configuration parameters

### General information 

* `gcs_bucket_name`: The name of the GCS bucket that you want audio to be uploaded to
* `project_id`: A name for the project
* `config_id`: A way to track device configurations (must be manually changed)

The values of `project_id` and `config_id` will be included in the filename of uploaded audio data on the remote server.

### Audio recording parameters 

* `record_length`: Audio file size (seconds)
* `capture_delay`: Delay between recording audio files (seconds)
* `record_freq`: Audio sampling frequency (Hz)
* `compress_data`: Enable/disable MP3 VBR0 audio compression
* `sensor_type`: "I2SMic" for the onboard MEMS mic, "XXX" for external microphones

See the [audio](audio.md) page for more detail on Bugg audio specifications.

### Mobile internet settings

These settings are specific to both the network provider and mobile data plan used by the nano SIM. 

* `hostname`: APN hostname
* `username`: APN username
* `password`: APN password 

They can easily be found by searching online (e.g., "Vodafone UK pay monthly APN settings"), or by inserting the SIM card into a phone and finding the APN details in the relevant settings page of the phone OS.

See the [connectivity](connectivity.md) page for more details on mobile internet settings.

### Remote server details

Bugg devices upload audio data to a Google Cloud Storage bucket. The authentication details in the configuration file allow the Bugg to write data to the bucket through a service account.

Once you have [set up]((connectivity.md#google-cloud-storage-bucket)) a GCS bucket and a service account with the required permissions, create a new service account key on the GCS console and download it as a JSON file.

From the service account key, copy the fields `type`, `project_id`, `private_key_id`, `private_key`, `client_email`, `client_id`, `auth_uri`, `token_uri`, `auth_provider_x509_cert_url`, and `client_x509_cert_url` to the Bugg configuration file.