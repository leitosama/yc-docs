# Image search response format

In response to an image search query, {{ search-api }} returns an XML file encoded in [UTF-8](https://en.wikipedia.org/wiki/UTF-8) containing the search results.

XML files consist of the [request](#request-el) (aggregate information on search query parameters) and [response](#response-el) (search query handling results) grouping tags.

Below is the general structure of the resulting XML document with examples of values.

## Response structure {#response-format}

```xml
<?xml version="1.0" encoding="utf-8"?>
<yandexsearch version="1.0">
<request>
   <query>cats</query>
   <page>2</page>
   <sortby order="descending" priority="no">rlv</sortby>
   <maxpassages>1</maxpassages>
   <groupings>
      <groupby attr="ii" mode="deep" groups-on-page="1" docs-in-group="1" curcateg="-1"/>
   </groupings>
</request>
<response date="20240414T091102">
   <reqid>1713085862371215-3284531111984749627-balancer-l7leveler-kubr-yp-vla-25-BAL</reqid>
   <found priority="phrase">11132</found>
   <found priority="strict">11132</found>
   <found priority="all">11132</found>
   <found-human>11,000 responses found</found-human>
   <results>
      <grouping attr="ii" mode="deep" groups-on-page="1" docs-in-group="1" curcateg="-1">
         <page first="1" last="1">2</page>
         <group>
            <categ attr="ii" id="3166497932182189502"/>
            <doccount>1</doccount>
            <relevance priority="all"/>
            <doc id="Z37E83657D25C5DED">
               <relevance priority="all"/>
               <url>https://***.*****.**/big3/989/426616-Kycb.jpg</url>
               <domain>***.*****.**</domain>
               <modtime>20170920T100608</modtime>
               <size>0</size>
               <charset>utf-8</charset>
               <image-properties>
                  <id>63966cd66b86c8e11bf80c4c7dc5e431c5cd1b5d-8971534-images-thumbs</id>
                  <shard>0</shard>
                  <thumbnail-link>http://avatars.mds.yandex.net/i?id=63966cd66b86c8e11bf80c4c7dc5e431c5cd1b5d-8971534-images-thumbs</thumbnail-link>
                  <thumbnail-width>480</thumbnail-width>
                  <thumbnail-height>272</thumbnail-height>
                  <original-width>480</original-width>
                  <original-height>272</original-height>
                  <html-link>https://****.*********.ru/download/kot-kotyara-usy-lapy-hvost-2399/480x272/</html-link>
                  <image-link>https://****.*********.ru/original/480x272/8/d3/kot-kotyara-usy-lapy-hvost-2399.jpg</image-link>
                  <file-size>57435</file-size>
                  <mime-type>jpg</mime-type>
               </image-properties>
               <mime-type>text/html</mime-type>
               <properties/>
            </doc>
         </group>
         <found priority="phrase">8432</found>
         <found priority="strict">8432</found>
         <found priority="all">8432</found>
         <found-docs priority="phrase">8432</found-docs>
         <found-docs priority="strict">8432</found-docs>
         <found-docs priority="all">8432</found-docs>
         <found-docs-human>8,000 responses found</found-docs-human>
      </grouping>
   </results>
</response>
</yandexsearch>
```


## Response parameters {#response-parameters}

The `request` group provides aggregate information about request parameters. It may be missing if there are errors in the response.

### request {#request-el}

#|
|| **`request` group tags** | **Description** | **Attributes** ||
|| query | Text of the sent search query | N/A ||
|| page | Number of the returned page with search results. Page numbering starts from zero (the `0` value stands for page 1). | N/A ||
|| sortby |
Result sorting parameters. Service tag, sets to `rlv`: sorting by relevance.
|
* `order`: Sorting order. Service attribute, sets to `descending` (direct sorting order).
* `priority`: Service attribute. Sets to `no`.
||
|| maxpassages | Maximum number of text snippets generated for each image. Service tag, sets to `1`. | Missing||
|| groupings |
Grouping tag.

Contains grouping parameters in the `groupby` tag.
| Missing ||
|| groupby | Parameters for grouping found search results |
* `attr`: Service attribute, sets to `ii`.
* `mode`: Grouping method. Service attribute, sets to `deep`.
* `groups-on-page`: Maximum number of search result groups that can be returned per results page. If the attribute is not specified, the default value is `20`.
* `docs-in-group`: Maximum number of images that can be returned per group. Service attribute, sets to `1`.
* `curcateg`: Service attribute. Sets to `-1`.
||
|#

### response {#response-el}

Results of processing the search query, information about which is provided in the [request](#request-el) tag.

It contains the `date` attribute with the query date and time (UTC) in `<year><month><day>Т<hour><minute><second>` format.

This group consists of the following sections:

* [General information about search results](#basic-search-info)
* [`results` section](#results-block)

#### General information about search results {#basic-search-info}

The table below lists the tags used in the appropriate section.

#|
|| **Tags for general information about search results** | **Description** | **Attributes** ||
|| error |
Error description.

Used only if a search query is handled incorrectly (e.g., if the query is empty or parameters are incorrect).

In some cases, the tag is mutually exclusive with other tags of the `response` grouping tag.
| `code`: Error [code](../reference/error-codes.md) ||
|| reqid | Unique ID of the query | Missing ||
|| found | Estimate of the number of images found in response to the query
|
`priority`: Service attribute. The possible values include:

* `phrase`
* `strict`
* `all`
||
|| found-human | String in the language matching the selected [search type](../operations/registration.md). Contains information about the number of found images and related information. | Missing ||
|#


#### results {#results-block}

This section is optional and only used if any results are found for a query.

The table below lists the tags for this section.

#|
|| **`results` section tags** | **Description** | **Attributes**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ||
|| results | Grouping tag. Child tags contain information about search parameters and found images. | Missing ||
|| grouping |

Grouping tag. Child tags contain information about search parameters and found images.

|
Attributes show rules for grouping the found images.

* `attr`: Service attribute, sets to `ii`.
* `mode`: Grouping method. Service attribute, sets to `deep`.
* `groups-on-page`: Maximum number of search result groups that can be returned per results page. If the parameter is not specified, the default value is `20`.
* `docs-in-group`: Maximum number of images that can be returned per group. Service attribute, sets to `1`.
* `curcateg`: Service attribute. Sets to `-1`.
||
|| page | Number of the returned page with search results. Page numbering starts from zero (the `0` value stands for page 1).
|
* `first`: Number of the first group with search results displayed on the page.
* `last`: Number of the last group with search results displayed on the page.
||
|| group |
Grouping tag.

Each `group` tag contains information about one group of found images. Since each group of images contains one image, the tag provides information about one image.
| Missing
||
|| categ | Description of the found image |
* `attr`: Group name. Matches the value of the `attr` attribute of the `groupby` tag of the `request` element.
* `id`: ID of the image host
||
|| doccount | Service tag | Missing ||
|| relevance | Service tag |`priority`: Service attribute ||
|| doc |
Grouping tag.

Each `doc` tag contains information about one found image.

| `id`: Unique ID of the found image
||
|| relevance | Service tag |`priority`: Service attribute ||
|| url | Image address | Missing ||
|| domain | Domain that hosts the document containing the image | Missing ||
|| modtime | Date and time the image was modified in `<year><month><day>T<hour><minute><second>` format in UTC | Missing ||
|| size | Image size in bytes | Missing ||
|| charset | Encoding of the document containing the image | Missing ||
|| image-properties |
Grouping tag.

Contains information about the image properties to include in search results.

| Missing
||
|| id | Image thumbnail ID | Missing ||
|| shard | Number of the shard containing the image information | Missing ||
|| thumbnail-link | Image thumbnail address | Missing ||
|| thumbnail-width | Width of the image thumbnail in pixels | Missing ||
|| thumbnail-height | Height of the image thumbnail in pixels | Missing ||
|| original-width | Width of the source (original) image | Missing ||
|| original-height | Height of the source (original) image | Missing ||
|| html-link | Image page address | Missing ||
|| image-link | Image address | Missing ||
|| file-size | Image size in bytes | Missing ||
|| mime-type | Image format (JPG, GIF, or PNG) | Missing ||
|| mime-type | Format of the document containing the image | Missing ||
|| found | Estimate of the number of generated groups |
`priority`: Service attribute. The possible values include:

* `phrase`
* `strict`
* `all`
||
|| found-docs |

Estimate of the number of images found in response to the query.

It is a more accurate estimate as compared to the value provided in the `found` tag of the [general information about search results](#basic-search-info).

|
`priority`: Service attribute. The possible values include:

* `phrase`
* `strict`
* `all`
||
|| found-docs-human |
String in the language matching the selected [search type](../operations/registration.md). Contains information about the number of found images and related information.

The value being provided must be used when generating search results.

| Missing
||
|#

#### See also {#see-also}

* [{#T}](./pic-search.md)
* [{#T}](../operations/searching.md)