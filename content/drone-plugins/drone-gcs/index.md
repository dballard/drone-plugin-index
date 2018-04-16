---
date: 2016-01-01T00:00:00+00:00
title: Google Cloud Storage
author: drone-plugins
tags: [ google, gcp, gcs, storage ]
repo: drone-plugins/drone-gcs
logo: google_gcs.svg
image: plugins/gcs
---

The GCS plugin uploads files and build artifacts to your GCS bucket. The below pipeline configuration demonstrates simple usage:

```yaml
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
    secrets: [google_credentials]
```

Excluding a directory from uploading:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
+   ignore: bin/*
    secrets: [google_credentials]
```

Configuring an ACL for the uploaded files:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
+   acl:
+     - allUsers:READER
    secrets: [google_credentials]
```

Configuring gzip compression for the uploaded files:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
+   gzip:
+     - js
+     - css
+     - html
    secrets: [google_credentials]
```

Configuring cache control headers for the uploaded files:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
+   cache_control: public,max-age=3600
    secrets: [google_credentials]
```

Configuring metadata headers for the uploaded files:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
    metadata:
      x-goog-meta-foo: bar
    secrets: [google_credentials]
```

Example configuration using inline credentials:

```diff
pipeline:
  gcs:
    image: plugins/gcs
    source: dist
    target: bucket/dir/
-   secrets: [google_credentials]
+   token: >
+     {
+       "type": "service_account",
+       "project_id": "xxx",
+       "private_key_id": "xxx",
+       "private_key": "xxx",
+       "client_email": "xxx",
+       "client_id": "xxx",
+       "auth_uri": "https://accounts.google.com/o/oauth2/auth",
+       "token_uri": "https://accounts.google.com/o/oauth2/token",
+       "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
+       "client_x509_cert_url": "xxx"
+     }
```

# Secret Reference

google_credentials, token
: [GCP service account](https://developers.google.com/console/help/new/#serviceaccounts) JSON credential for authentication

# Parameter Reference

source
: location of files to upload

target
: destination to copy files to, including bucket name

ignore
: skip files matching this [pattern](https://golang.org/pkg/path/filepath/#Match), relative to source (optional)

acl
: list of access rules applied to the uploaded files, in a form of [`entity:role`](https://cloud.google.com/storage/docs/access-control/lists) (optional)

gzip
: list of file extensions to gzip and upload with the `Content-Encoding: gzip` header (optional)

cache_control
: `Cache-Control` header value (optional)

metadata
: arbitrary dictionary of custom [metadata](https://cloud.google.com/storage/docs/metadata) to apply to all objects (optional)
