# Pre-signed URLs

{% include [full-overview](./full-overview.md) %}

With pre-signed URLs, any web user can perform various operations in {{ objstorage-name }}, such as:

* Download an object.
* Upload an object.
* Create a bucket.

A pre-signed URL is a URL containing request authorization data in its parameters. Pre-signed URLs can be created by users with static access keys.

This section outlines the general principles for generating pre-signed URLs using [AWS Signature V4](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html).

{% note info %}

SDKs for various programming languages and other tools for AWS S3 feature out-of-the-box methods for generating pre-signed URLs that you can also use for {{ objstorage-name }}.

{% endnote %}

## General pre-signed URL format {#presigned-url-preview}

```
https://<bucket_name>.{{ s3-storage-host }}/<object_key>?
     X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<access_key-id>%2F<YYYYMMDD>%2F{{ region-id }}%2Fs3%2Faws4_request
    &X-Amz-Date=<time_in_ISO_8601_format>
    &X-Amz-Expires=<URL_lifetime>
    &X-Amz-SignedHeaders=<list_of_signed_headers>
    &X-Amz-Signature=<signature>
```

Pre-signed URL parameters:

| Parameter | Description |
---------|---------
| `X-Amz-Algorithm` | Identifies the signature version and algorithm for its calculation. Value: `AWS4-HMAC-SHA256`. |
| `X-Amz-Credential` | Signature ID.<br/><br/>This is a string in `<access-key-id>/<YYYYMMDD>/{{ region-id }}/s3/aws4_request` format, where `<YYYYMMDD>` must match the date set in the `X-Amz-Date` header. |
| `x-amz-date` | Time in [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) format, e.g., `20180719T000000Z`. The date specified must match the date in the `X-Amz-Credential` parameter (by value rather than by format). |
| `X-Amz-Expires` | Link validity time in seconds. The starting point is the time specified in `X-Amz-Date`. The maximum value is 2592000 seconds (30 days). |
| `X-Amz-SignedHeaders` | Headers of the request you want to sign, delimited by a semicolon (`;`).<br/><br/>Make sure to sign the `Host` header and all `X-Amz-*` headers used in the request. You do not have to sign other headers; however, the more headers you sign, the safer your request is going to be. |
| `X-Amz-Signature` | Request signature. |

## Creating pre-signed URLs {#creating-presigned-url}

{% note info %}

Generating pre-signed URLs is optional for public buckets. You can get files from a publicly available bucket via both HTTP and HTTPS even if the bucket has no [website hosting](../../../storage/concepts/hosting.md) configured.

{% endnote %}

To get a pre-signed URL:

1. [Create a canonical request](#canonical-request).
1. [Compose a string to sign](#composing-string-to-sign).
1. [Generate a signing key](#signing-key-gen).
1. [Calculate the signature using the key](#signing).
1. [Generate a pre-signed URL](#composing-signed-url).

To create a pre-signed URL, you must have [static access keys](../../../iam/operations/sa/create-access-key.md).

### Canonical request {#canonical-request}

The general canonical request format is as follows:

```
<HTTPVerb>\n
<CanonicalURL>\n
<CanonicalQueryString>\n
<CanonicalHeaders>\n
<SignedHeaders>\n
UNSIGNED-PAYLOAD
```

#### HTTPVerb {#http-verb}

HTTPVerb stands for the HTTP method used to send a request: `GET`, `PUT`, `HEAD`, or `DELETE`.

#### CanonicalURL {#canonical-url}

URL-encoded object key. For example, `/folder/object.ext`.

{% note info %}

Do not normalize the path. For example, if an object has a `some//strange//key//example` key, normalizing the path to `/<bucket-name>/some/strange/key/example` will invalidate it.

{% endnote %}

#### CanonicalQueryString {#canonical-query-string}

The canonical query string must include all query parameters of the destination URL, except `X-Amz-Signature`. The parameters in the string must be URL-encoded and sorted alphabetically.

For example:

```
X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=JK38EXAMPLEAKDID8%2F20190801%2F{{ region-id }}%2Fs3%2Faws4_request&X-Amz-Date=20190801T000000Z&X-Amz-Expires=86400&X-Amz-SignedHeaders=host
```

#### CanonicalHeaders {#canonical-headers}

This section includes the list of the request headers and their values.

The requirements are as follows:

* Each header must be separated with the `\n` newline character.
* Header names must be lowercase.
* Headers must be sorted alphabetically.
* There may not be any extra spaces.
* The list must contain the `host` header and all `x-amz-*` headers used in the request.

You can also add any request header to the list. The more headers you sign, the safer your request is going to be.

For example:

```
host:sample-bucket.{{ s3-storage-host }}
x-amz-date:20190801T000000Z
```

#### SignedHeaders {#signed-headers}

This is a list of lowercase request header names, sorted alphabetically and separated by semicolons.

For example:

```
host;x-amz-date
```

#### Canonical request ending {#end-canonical-request}

A canonical request must always end with an `UNSIGNED-PAYLOAD` string.

### String to sign {#composing-string-to-sign}

General format of a string to sign:

```
"AWS4-HMAC-SHA256" + "\n" +
<timestamp> + "\n" +
<scope> + "\n" +
Hex(Hash-SHA256(<CanonicalRequest>))
```

Where:

* `AWS4-HMAC-SHA256`: Hashing algorithm.
* `timestamp`: Current time in ISO 8601 format, for example, `20190801T000000Z`. The specified date must match the date in `scope` (by value rather than by format).
* `scope`: `<YYYYMMDD>/{{ region-id }}/s3/aws4_request`.
* `CanonicalRequest`: [Canonical request](#canonical-request) generated earlier. The signature string contains the [SHA256](https://en.wikipedia.org/wiki/SHA-2) hash of the canonical request in hexadecimal representation.

### Signing key {#signing-key-gen}

{% include [generate-signing-key](../generate-signing-key.md) %}

### Sign a string with a key {#signing}

To get a string signature, use `HMAC` with the `SHA256` hash function and convert the result to hexadecimal format.

```
signature = Hex(sign(SigningKey, StringToSign))
```

### Pre-signed URLs {#composing-signed-url}

To compose a pre-signed URL, add the [parameters](#presigned-url-preview) required to authorize the request to the {{ objstorage-name }} resource URL, including the `X-Amz-Signature` parameter containing the calculated signature.

The other parameter values must match their respective values specified earlier in the [canonical request](#canonical-request) and the [signature string](#composing-string-to-sign).

#### Example of composing a pre-signed URL for downloading an object {#example-for-object-download}

Let's put together a pre-signed URL to download the `object-for-share.txt` object from the bucket:

- Static key:

   ```
   access_key_id = 'JK38EXAMP********'
   secret_access_key = 'ExamP1eSecReTKeykdo********'
   ```

- Canonical request:

   ```
   GET
   /object-for-share.txt
   X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=YCAJEK0Iv6x********eLTAdg%2F20231208%2F{{ region-id }}%2Fs3%2Faws4_request&X-Amz-Date=20231208T184504Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host
   host:<bucket_name>.{{ s3-storage-host }}

   host
   UNSIGNED-PAYLOAD
   ```

- String to sign:

   ```
   AWS4-HMAC-SHA256
   20231208T184504Z
   20231208/{{ region-id }}/s3/aws4_request
   e823d75aad02c1317589bd5373fe9e20d5ef44499237703ff23e5600********
   ```

- Signing key:

   ```
   sign(sign(sign(sign("AWS4" + "ExamP1eSecReTKeykdokKK38800","20190801"),"{{ region-id }}"),"s3"),"aws4_request")
   ```

   Here, we introduce the `sign` function to indicate the method of key calculation that uses the [HMAC](https://ru.wikipedia.org/wiki/HMAC) algorithm with [SHA256](https://ru.wikipedia.org/wiki/SHA-2).

- Signature:

   ```
   b10c16a1997bb524bf59974512f1a6561cf2953c29dc3efbdb920790********
   ```

- Pre-signed URL:

   ```
   https://<bucket_name>.{{ s3-storage-host }}/object-for-share.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=YCAJEK0Iv6xqy-pEQcueLTAdg%2F20231208%2F{{ region-id }}%2Fs3%2Faws4_request&X-Amz-Date=20231208T195434Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=b10c16a1997bb524bf59974512f1a6561cf2953c29dc3efbdb920790********
   ```

#### Code examples for generating pre-signed URLs {#code-examples}

{% list tabs %}

- PHP

   ```php
   <?php

     $keyid = "<static_key_ID>";
     $secretkey = "static_key_contents";
     $path = "<object_key>";
     $objectname = implode("/", array_map("rawurlencode", explode("/", $path)));
     $host = "<bucket_name>.storage.yandexcloud.net";
     $region = "ru-central1";
     $timestamp = time();
     $dater = strval(date('Ymd', $timestamp));
     $dateValue = strval(date('Ymd', $timestamp))."T".strval(date('His', $timestamp))."Z";

     // Generate the canonical request
     $canonical_request = "GET\n".$objectname."\nX-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=".$keyid."%2F".$dater."%2Fru-central1%2Fs3%2Faws4_request&X-Amz-Date=".$dateValue."&X-Amz-Expires=3600&X-Amz-SignedHeaders=host\nhost:".$host."\n\nhost\nUNSIGNED-PAYLOAD";

     echo "<b>Canonical request: </b><br>".$canonical_request."<br><br>";

     // Generate the string to be signed
     $string_to_sign = "AWS4-HMAC-SHA256\n".$dateValue."\n".$dater."/".$region."/s3/aws4_request\n".openssl_digest($canonical_request, "sha256", $binary = false);

     echo "<b>String to be signed: </b><br>".$string_to_sign."<br><br>";

     // Generate the signing key
     $signing_key = hash_hmac('sha256', 'aws4_request', hash_hmac('sha256', 's3', hash_hmac('sha256', 'ru-central1', hash_hmac('sha256', $dater, 'AWS4'.$secretkey, true), true), true), true);

     echo "<b>Signing key: </b><br>".$signing_key."<br><br>";

     // Generate the signature
     $signature = hash_hmac('sha256', $string_to_sign, $signing_key);

     echo "<b>Signature: </b><br>".$signature."<br><br>";

     // Generate the pre-signed link
     $signed_link = "https://".$host.$objectname."?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=".$keyid."%2F".$dater."%2Fru-central1%2Fs3%2Faws4_request&X-Amz-Date=".$dateValue."&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=".$signature."\n";

     echo '<b>Signed link: </b><br>'.'<a href = "'.$signed_link.'" target = "_blank">'.$signed_link.'</a>';

   ?>
   ```

{% endlist %}

## Examples of getting pre-signed links in {{ objstorage-name }} tools {#example-for-getting-in-tools}

{% list tabs %}

- Management console {#console}

   {% include [storage-get-link-for-download](../../../storage/_includes_service/storage-get-link-for-download.md) %}

- AWS CLI {#cli}

   You can also use AWS CLI to generate a link for downloading an object. To do this, run the following command:

   ```
   aws s3 presign s3://<bucket-name>/<object-key> --endpoint-url "https://{{ s3-storage-host }}/" [--expires-in <value>]
   ```

   To generate the link properly, make sure to provide the `--endpoint-url` parameter pointing to the {{ objstorage-name }} hostname. For detailed information, see [this section covering AWS CLI specifics](../../../storage/tools/aws-cli.md#specifics).

- Python (boto3) {#boto3}

   The example generates a pre-signed URL for downloading the `object-for-share` object from the `bucket-with-objects` bucket. The URL is valid for 100 seconds.

   ```python
   # coding=utf-8

   import boto3
   from botocore.client import Config


   ENDPOINT = "https://{{ s3-storage-host }}"

   ACCESS_KEY = "JK38EXAMP********"
   SECRET_KEY = "ExamP1eSecReTKeykdo********"

   session = boto3.Session(
       aws_access_key_id=ACCESS_KEY,
       aws_secret_access_key=SECRET_KEY,
       region_name="{{ region-id }}",
   )
   s3 = session.client(
       "s3", endpoint_url=ENDPOINT, config=Config(signature_version="s3v4")
   )

   presigned_url = s3.generate_presigned_url(
       "get_object",
       Params={"Bucket": "bucket-with-objects", "Key": "object-for-share"},
       ExpiresIn=100,
   )

   print(presigned_url)
   ```

{% endlist %}
