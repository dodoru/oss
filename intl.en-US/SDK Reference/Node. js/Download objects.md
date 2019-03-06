# Download objects {#concept_32073_zh .concept}

This topic describes how to download objects.

You can download a file from OSS using any of the following methods:

-   Download an object to a local file
-   Stream download
-   Down an object to local buffer
-   HTTP download \(by a browser\)

## Download an object to a local file {#section_mdn_dsk_lfb .section}

The following code uses the get interface to download an object as a local file:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function get () {
  try {
    let result = await client.get('object-name', 'local-file');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

get();

```

## Stream download { .section}

When you use `getStream` to download an object, a `Readable Stream` is returned for you to process object content

```language-js
let OSS = require('ali-oss');
let fs = require('fs');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function getStream () {
  try {
    let result = await client.getStream('object-name');
    console.log(result);
    let writeStream = fs.createWriteStream('local-file');
    result.stream.pipe(writeStream);
  } catch (e) {
    console.log(e);
  }
}

getStream()

```

## Download an object to local buffer { .section}

You can also download the object content to the buffer through the `get` interface simply:

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

async function getBuffer () {
  try {
    let result = await client.get('object-name');
    console.log(result.content);
  } catch (e) {
    console.log(e);
  }
}

getBuffer();

```

## HTTP download { .section}

Objects stored in OSS can be downloaded through HTTP directly without using the SDK. HTTP download supports browsers and command-line interface \(CLI\) tools \(such as `wget` and `curl`\). The URL of the object to be downloadedis generated by the SDK. The `signatureUrl` method is used to generate an HTTP download address. The valid period of a URL is 30 minutes by default.

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

let url = client.signatureUrl('object-name');
console.log(url);

let url = client.signatureUrl('object-name', {expires: 3600});
console.log(url);

// signed URL for PUT
let url = client.signatureUrl('object-name', {method: 'PUT'});
console.log(url);

```
