---
tags: [Google Cloud Platform]
title: Google Cloud Storage gzipping
created: '2023-08-07T08:40:39.192Z'
modified: '2023-08-07T09:13:50.341Z'
---

# Google Cloud Storage gzipping
The gzip option must be set on `.save`, on `.download` decompressing is default.

## Common
```js
import { Storage } from '@google-cloud/storage';
const storage = new Storage({ projectId: process.env.GCP_PROJECT_ID });
const myBucket = storage.bucket('my-bucket');
const file = myBucket.file('my-file');
```

## Upload a gzipped file
```js
await file.save('some-file-contents', { resumable: false, gzip: true });
```
- [.save() options](https://googleapis.dev/nodejs/storage/latest/global.html#CreateWriteStreamOptions)


## Check it's gzipped
```js
it('is gzipped', async() => {
  const [metadata] = await file.getMetadata();
  assert.equal(metadata.contentEncoding, 'gzip');
});
```
- [.getMetadata()](https://googleapis.dev/nodejs/storage/latest/File.html#getMetadata)
- [gzip option sets metadata.contentEncoding](https://googleapis.dev/nodejs/storage/latest/global.html#CreateWriteStreamOptions)

## Download
```js
const [fileContents] = await file.download();
// fileContents === 'some-file-contents'
```

