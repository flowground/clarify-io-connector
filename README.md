# ![LOGO](logo.png) api.clarify.io **flow**ground Connector

## Description

A generated **flow**ground connector for the api.clarify.io API (version 1.3.7).

Generated from: https://api.apis.guru/v2/specs/clarify.io/1.3.7/swagger.json<br/>
Generated at: 2019-05-07T17:39:59+03:00

## API Description

The API to Search and Understand Audio & Video Data.

## Authorization

This API does not require authorization.

## Actions

### List bundles

> Gets the list of bundles. Links to each item are in the _links with link relation <b>items</b>.<br/><br/>After getting the initial list, use the <b>first</b>, <b>last</b>, <b>next</b>, <b>prev</b> link relations to get more bundles in the list. Note that <b>next</b> will not be available at the end of the list and <b>prev</b> will not be available at the start of the list. If the results are exactly one page neither <b>prev</b> nor <b>next</b> will be available.<br/><br/>The <b>embed</b> parameter specifies link relations to embed in the results. The models for the specified link relations will be in an array in the embedded object with the link relation as the key. For example, if you do embed=items, _embedded will contain a property <b>items</b> whose value is the array of bundle models. For link relations that are curies (ex. "clarify:metadata"), you may simply use the base name (ex. "metadata").

*Tags:* `bundles`

#### Input Parameters
* `limit` - _optional_ - limit results to specified number of bundles. Default is 10. Max 100.
* `embed` - _optional_ - list of link relations to embed in the result collection. Zero or more of: items, tracks, metadata, insights. List is space or comma separated single string or an array of strings
* `iterator` - _optional_ - optional opaque value, automatically provided in next/prev links, or literal "first", "last"

### Create a bundle

> Create a new bundle with the specified name, media url, and optional JSON metadata.<br/><br/><b>name</b> can be any string you wish to associate with the bundle.<br/><br/><b>media_url</b> must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.<br/><br/><b>audio_channel</b> is used to specify audio channels if the media is a stereo file. A value of <i>left</i> or <i>right</i> signifies that only the specified channel will be used. If no value or an empty string is specified for <b>audio_channel</b>, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks, with <b>audio_channel</b> set to <i>left</i> and <i>right</i> in each track respectively. If your stereo file is simply a recording made with a stereo microphone, <b>audio_channel</b> should be set to an empty string (or not be specified.) If you have audio channels as separate media files, after creating the bundle with one <b>media_url</b>, POST another <b>media_url</b> to /bundles/{bundle_id}/tracks.<br/><br/><b>audio_language</b> can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see <a href="http://tools.ietf.org/html/rfc5646" target="_blank">http://tools.ietf.org/html/rfc5646</a>). Supported languages: en-US, en-UK, es, fr.<br/><br/><b>label</b> is a short name for the track.<br/><br/><b>metadata</b> is a single-level JSON object of your own definition, containing key-values that can be searched and filtered on. Metadata can be used to hold text such as names, titles, descriptions and values for segregating bundles, for example by user, topic, folder name etc. The keys (property names) can be up to 64 characters and must contain only alphanumeric characters and underscore (but not start with underscore) and must not be a reserved name. Reserved names are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;. Values can be strings, numbers, boolean true/false, date-times represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an array of these primitive types. Strings can be up to 2000 characters and strings in arrays can be up to 128 characters each. Nested objects are not allowed. Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000 characters.<br/><br/><b>start_time</b> a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.<br/><br/><b>parts_pending</b> a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.<br/><br/><b>external_id</b> is an optional parameter that can be used to logically link a bundle to an item in an external system. The <b>external_id</b> can be whatever you use to identify items in your own database.<br/><br/><b>notify_url</b> is a webhook. It must be a publicly accessible url (http or https) on your server to which notifications for the bundle will be POSTed. There are three types of notifications: Track Notifications, Insight Notifications and Bundle Notifications. For more information on the content of notifications and when they are sent, see the <a href="http://docs.clarify.io/overview/#notifications" target="clarify">notification docs page</a>.<br/><br/>If a track was created along with the budle, the link relation <b>clarify:track</b> will be included with a link to the new track.

*Tags:* `bundles`

### Delete a bundle

> Delete a bundle and its related metadata and tracks. This will only delete media stored on Clarify systems and not delete the source media on remote systems.<br/><br/>Successful response will be a HTTP code 204 with an empty body.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Get a bundle

> Get a bundle that has previously been created.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle
* `embed` - _optional_ - list of link relations to embed in the result bundle. Zero or more of: tracks, metadata, insights. List is space or comma separated single string or an array of strings

### Update a bundle

> Update a bundle. To update the tracks, media, or metadata of a bundle, use the tracks and metadata endpoints.<br/><br/><b>name</b> can be any string you wish to associate with the bundle.<br/><br/><b>external_id</b> is an optional parameter that can be used to logically link a bundle to an item in an external system. The <b>external_id</b> can be whatever you use to identify items in your own database.<br/><br/><b>notify_url</b> is a webhook. It must be a publicly accessible url (http or https) on your server to which notifications for the bundle will be POSTed. There are three types of notifications: Track Notifications, Insight Notifications and Bundle Notifications. For more information on the content of notifications and when they are sent, see the <a href="http://docs.clarify.io/overview/#notifications" target="clarify">notification docs page</a>.<br/><br/>If <b>version</b> is specified, the bundle will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the bundle will always be updated.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Get bundle insights

> Gets the insights for a bundle.<br/><br/>URLs of the available insights for the bundle are in the _links object, with the link relations (keys) of the format <b>insight:insight_name</b>.<br/><br/>Documentation on the insights available and the data returned can be found at <a target="clarify" href="http://docs.clarify.io/insights/">http://docs.clarify.io/insights/</a>

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Request an insight to be run

> Request an insight to be run on a bundle. Note that most insights are set to automatically run on all bundles so you commonly won&apos;t need to call this endpoint except to request transcripts. To configure which insights are automatically run for an app, visit the <a href="https://developer.clarify.io" target="clarify">Clarify Developer Portal</a>.<br/><br/> Insights that are not configured to autorun can be requested to run on an individual bundle using this endpoint. The following insights can be requested:<br/><br/><b>transcript_r9</b> - High-accuracy transcript of the speech in audio media.<br/><br/>Transcripts will produced on the mixed audio of all tracks in the bundle and are charged per minute (rounded up for partial minutes), based on the duration of the longest track. If the request has already been made, this method has no effect other than to return the existing insight.<br/><br/>Transcripts will typically take about 48 hours. When the transcript is ready, an InsightNotification webhook will be POSTed to the bundle <b>notify_url</b>.<br/><br/>For more information see <a href="http://docs.clarify.io/quickstarts/human-transcripts.html" title="human transcripts" target="clarify">Human Transcripts Quick Start</a>.<br/><br/><b>captions_r9</b> - High-accuracy captions of the speech in video media.<br/><br/>Captions will be generated on the first track in the bundle. and are charged per minute (rounded up for partial minutes), based on the duration of the media.  See the <a href="http://clarify.io/pricing" title="pricing" target="clarify">pricing page</a>. If the request has already been made, this method has no effect other than to return the existing insight.<br/><br/>Captions will typically take about 72 hours. When the captions are ready, an InsightNotification webhook will be POSTed to the bundle <b>notify_url</b>.<br/><br/>For more information see <a href="http://docs.clarify.io/quickstarts/closed-captions.html" title="captions" target="clarify">Captions Quick Start</a>.<br/><br/><b>spoken_keywords</b> - Spoken words of interest found in audio media. <b>Note:</b> Normally spoken_keywords is set to autorun so you do not need to run it explicitly.<br/><br/><b>spoken_topics</b> - Topics spoken about in the audio media.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Get bundle insight

> Gets a particular insight for a bundle. Typically, you will hit this endpoint from a link contained in a response to <b>/v1/bundles/{bundle_id}/insights</b><br/><br/>The insight response may contain a <b>data</b> object containing insight-specific data and/or an array of objects called <b>track_data</b>, where the array indexes correspond to the tracks in the bundle. Each object in the array contains the <b>track_id</b>, <b>track_label</b> and insight-specific data related to that insight. For example, in the <b>spoken_words</b> insight, the <b>track_data</b> objects contain the field <b>word_count</b> which is the number of spoken words found in the track.<br/><br/>Documentation on the insights available and the data returned can be found at <a target="clarify" href="http://docs.clarify.io/insights/">http://docs.clarify.io/insights/</a><br/><br/>Insights that contain data in different file formats (such as for video captions) will have one or more link relations in the _links array for the corresponding data. Note that the href URLs in these links have a limited lifespan and should not be stored locally.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle
* `insight_id` - _required_ - id of an insight

### Delete bundle metadata

> Delete the metadata of a bundle and set data to {} (empty object.) This is functionally equivalent to an update metadata request with data set to {}.<br/><br/>Successful response will be a HTTP code 204 with an empty body.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Get bundle metadata

> Gets the metadata for a bundle.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Update bundle metadata

> Update the metadata for a bundle.<br/><br/>The metadata is a single-level JSON object of your own definition, containing key-values that can be searched and filtered on. Metadata can be used to hold text such as names, titles, descriptions and values for segregating bundles, for example by user, topic, folder name etc. The keys (property names) can be up to 64 characters and must contain only alphanumeric characters and underscore (but not start with underscore) and must not be a reserved name. Reserved names are &quot;true&quot;, &quot;false&quot;, and &quot;null&quot;. Values can be strings, numbers, boolean true/false, date-times represented as a string in ISO 8601 format (ex. &quot;2014-02-25T14:23:45.000Z&quot;), or an array of these primitive types. Strings can be up to 2000 characters and strings in arrays can be up to 128 characters each. Nested objects are not allowed. Metadata can contain up to 50 key-value pairs up to a total JSON size of 4000 characters.<br/><br/>To clear the metadata for a bundle, send <b>data</b>={}.<br/><br/>If <b>version</b> specified, the metadata will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict will be returned. If version not specified, the metadata will always be updated.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Delete bundle tracks

> Delete tracks of a bundle. This will only delete media stored on Clarify systems and not delete the source media on remote systems.<br/><br/>Successful response will be a HTTP code 204 with an empty body.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Get bundle tracks

> Gets the array of tracks for a bundle. This includes the specification of the media and the status of fetching and processing it.<br/><br/>Media for tracks is fetched asynchronously. Until media has been retrieved, a track&apos;s <b>duration</b> and <b>size</b> will both be set to -1.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Add a track for a bundle

> Add a new track to a bundle. This will insert or append a new track in the tracks array or return an error if the maximum number of tracks (12) has been reached or the track number specifies an invalid index.<br/><br/>Once all media parts have been added to a track it is immutable, meaning it cannot be modified. If you wish to modify a track, simply add a new one and delete the existing one.<br/><br/><b>label</b> is a short name for the track.<br/><br/><b>media_url</b> must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.<br/><br/><b>audio_channel</b> is used to specify audio channels if the media is a stereo file. A value of <i>left</i> or <i>right</i> signifies that only the specified channel will be used. If no value or an empty string is specified for <b>audio_channel</b>, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks with <b>audio_channel</b> to be <i>left</i> and <i>right</i>. If your stereo file is simply a recording made with a stereo microphone, <b>audio_channel</b> should be set to an empty string (or not be specified.)<br/><br/><b>audio_language</b> can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see <a href="http://tools.ietf.org/html/rfc5646" target="_blank">http://tools.ietf.org/html/rfc5646</a>). Supported languages: en-US, en-UK, es, fr.<br/><br/><b>start_time</b> a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.<br/><br/><b>parts_pending</b> a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.<br/><br/><b>track</b> is the index in the tracks array where the new track will be added. Track numbers start at 0. If this parameter is not specified the new track will always be appended to the end of the array. If the track specified is greater than the last index of the array + 1, an error will be returned.<br/><br/>If <b>version</b> specified, the track will only be added if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Update a tracks for a bundle

> Update tracks for a bundle.<br/><br/><b>parts_complete</b> a boolean <code>true</code> or <cade>false</code>. If true, any tracks in the PENDING state will be queued for processing and no more media parts may be added to the tracks. Default is false.<br/><br/>If <b>version</b> specified, the track will only be updated if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle

### Delete a bundle track

> Delete a track of a bundle. This will only delete media stored on Clarify systems and not delete the source media on remote systems.<br/><br/>Successful response will be a HTTP code 204 with an empty body.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle
* `track_id` - _required_ - id of a track

### Get bundle track

> Gets a single track in a bundle. This includes the specification of the media and the status of fetching and processing it.<br/><br/>Media for a track is fetched asynchronously. Until media has been retrieved, a track&apos;s <b>duration</b> and <b>size</b> will both be set to -1.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle
* `track_id` - _required_ - id of a track

### Add media to a track

> Add media to an existing track of a bundle. This can only be called on a track that currently has no media set or has parts pending.<br/><br/>Once all media parts have been added to a track it is immutable, meaning it cannot be modified. If you wish to modify a track, simply add a new one and delete the existing one.<br/><br/><b>media_url</b> must be a publicly accessible url to a media file. It will be fetched asynchronously after the REST call returns. The audio can be mono or stereo.<br/><br/><b>audio_channel</b> is used to specify audio channels if the media is a stereo file. A value of <i>left</i> or <i>right</i> signifies that only the specified channel will be used. If no value or an empty string is specified for <b>audio_channel</b>, all channels will be used in a single track. If your stereo channels were recorded separately with each channel containing distinct content (for example if 2 legs of a phone call were recorded separately and combined into a single stereo file), for best speech recognition, create two tracks with <b>audio_channel</b> to be <i>left</i> and <i>right</i>. If your stereo file is simply a recording made with a stereo microphone, <b>audio_channel</b> should be set to an empty string (or not be specified.)<br/><br/><b>audio_language</b> can be used to specify the language of the audio media. This is an optional parameter and if not specified or an empty string, the language of the track will be automatically detected. If specified, it must be a language code as described in RFC5646 (see <a href="http://tools.ietf.org/html/rfc5646" target="_blank">http://tools.ietf.org/html/rfc5646</a>). Supported languages: en-US, en-UK, es, fr.<br/><br/><b>start_time</b> a time in seconds that the media starts, relative to start time of the bundle. This allows you to specify sequential parts of media. If not specified, the default is 0.<br/><br/><b>parts_pending</b> a boolean flag specifying if more media parts will subsequently be added to the track. If true, a subsequent API call must be made to signify that the track is complete. If not specified, the default is false.<br/><br/>If <b>version</b> specified, the track will only be added if the current version matches this parameter value. If the version doesn't match, a 409 Conflict error will be returned. If version not specified, the track will always be updated.

*Tags:* `bundles`

#### Input Parameters
* `bundle_id` - _required_ - id of a bundle
* `track_id` - _required_ - id of a track

### Generate Group Report <span class="label">beta</span>

> Analyzes bundle content over a series of time periods grouped by the value of <b>group_field</b> metadata field and generates a report of top scores.<br/><br/><b>interval</b> specifies the duration of each time period in the report. For example, you can generate a report that gives monthly statistics. If there are no bundles for a given period, that period will not be present in the report.<br/><br/><b>score_field</b> specifies a bundle, insights, or metadata field to use as a score. The scores will be averaged across the group and listed in descending order.<br/><br/><b>group_field</b> specifies a metadata field by which to group statistics. Typically the field will represent a user or team id to get a report of the scores for the top users or teams.<br/><br/><b>filter</b> is used to limit the bundles in the report according to specific criteria based on metadata and bundle values.  A report filter behaves in the same way as a search filter. It uses an expression syntax similar to Javascript boolean expressions. An expression is made up of zero or more terms joined by logical operators with each term having a field, a comparison operator, and a literal value. Parentheses can be used to logically group terms.<br/><br/><div class="notes-indent">A filter term is of the form: <b><i><code>field-name comparison-operator literal-value</code></b></i> where:<br/><br/><b><i><code>field-name</code></i></b> is a metadata field or <code>bundle.name</code>, <code>bundle.id</code>, <code>bundle.external_id</code>, <code>bundle.created</code>, or <code>bundle.updated</code>.<br/><br/><b><i><code>comparison-operator</code></i></b> is <code>==</code>, <code>&lt;</code>, <code>&gt;</code>, <code>&lt;=</code>, <code>&gt=</code>, or <code>!=</code><br/><br/><b><i><code>literal-value</code></i></b> is a number (integer or decimal), boolean <code><i>true</i></code> or <code><i>false</i></code>, or a string with either double quotes (<code>"</code>) or single quotes (<code>'</code>).<br/><br/>Logical operators between terms (and groups of terms) can be <code>&&</code> (logical AND), <code>||</code> (logical OR). A logical NOT is <code>!</code> and can be placed before a term (or group of terms.)</div><br/><br/>An example filter expression (assuming you have used metadata fields category and tag): </p><br><div class="notes-indent"><code>category=="music" && (tag == "soft" || tag == "smooth") && tag != "jazz" && bundle.created > "2014-03-15T00:00:00.0Z"</code></div><br/><br/><p><b>language</b> parameter specifies the language to use for analyzing the report. This value is only relevant for language-related insight data. Supported languages: en, en-UK, en-US, es, fr.

*Tags:* `reports`

#### Input Parameters
* `interval` - _required_ - Duration of report periods. Default is month.
    Possible values: year, quarter, month, week, day, hour.
* `score_field` - _required_ - A bundle/metadata field to use as a score. Ex. insights.spoken_words.listener_score.
* `group_field` - _required_ - A metadata field by which to group scores, typically a user or team id field.
* `filter` - _optional_ - filter expression, typically programmatically generated based on input controls and data segregation rules etc. Up to 500 characters.
* `language` - _optional_ - Language to search in, specified with an RFC5646 code. Default is "en"
    Possible values: en, en-UK, en-US, es, fr.

### Generate Trends Report <span class="label">beta</span>

> Analyzes bundle content over a series of time periods and generates a trend report.<br/><br/><b>interval</b> specifies the duration of each time period in the report. For example, you can generate a report that gives monthly statistics. If there are no bundles for a given period, that period will not be present in the report.<br/><br/><b>content</b> specifies the content type to analyze and include in the report. These can include tracks and insights. Multiple values can be specified to generate a rich report. If <b>content</b> is not specified, only bundle counts are included in the report.<br/><br/><b>filter</b> is used to limit the bundles in the report according to specific criteria based on metadata and bundle values.  A report filter behaves in the same way as a search filter. It uses an expression syntax similar to Javascript boolean expressions. An expression is made up of zero or more terms joined by logical operators with each term having a field, a comparison operator, and a literal value. Parentheses can be used to logically group terms.<br/><br/><div class="notes-indent">A filter term is of the form: <b><i><code>field-name comparison-operator literal-value</code></b></i> where:<br/><br/><b><i><code>field-name</code></i></b> is a metadata field or <code>bundle.name</code>, <code>bundle.id</code>, <code>bundle.external_id</code>, <code>bundle.created</code>, or <code>bundle.updated</code>.<br/><br/><b><i><code>comparison-operator</code></i></b> is <code>==</code>, <code>&lt;</code>, <code>&gt;</code>, <code>&lt;=</code>, <code>&gt=</code>, or <code>!=</code><br/><br/><b><i><code>literal-value</code></i></b> is a number (integer or decimal), boolean <code><i>true</i></code> or <code><i>false</i></code>, or a string with either double quotes (<code>"</code>) or single quotes (<code>'</code>).<br/><br/>Logical operators between terms (and groups of terms) can be <code>&&</code> (logical AND), <code>||</code> (logical OR). A logical NOT is <code>!</code> and can be placed before a term (or group of terms.)</div><br/><br/>An example filter expression (assuming you have used metadata fields category and tag): </p><br><div class="notes-indent"><code>category=="music" && (tag == "soft" || tag == "smooth") && tag != "jazz" && bundle.created > "2014-03-15T00:00:00.0Z"</code></div><br/><br/><p><b>language</b> parameter specifies the language to use for analyzing the report. This value is only relevant for language-related insight data. Supported languages: en, en-UK, en-US, es, fr.

*Tags:* `reports`

#### Input Parameters
* `interval` - _required_ - Duration of report periods. Default is month.
    Possible values: year, quarter, month, week, day, hour.
* `content` - _optional_ - Content reported in each period. Zero or more of tracks, spoken_words, spoken_keywords. List is space or comma separated single string or an array of strings.
* `filter` - _optional_ - filter expression, typically programmatically generated based on input controls and data segregation rules etc. Up to 500 characters.
* `language` - _optional_ - Language to search in, specified with an RFC5646 code. Default is "en"
    Possible values: en, en-UK, en-US, es, fr.

### Search bundles

> Searches the bundles and returns a list of matching bundles, along with what matched and where for each bundle.<br/><br/><b>query</b> is used to search for text in the audio and metadata. It uses a simple query language similar to Google. At its simplest, it can be a space separated list of words (ex. <code>open voice</code>) which will find all bundles matching all the words. To search for a phrase, put it in quotes (ex. <code>"open source"</code>) You can exclude bundles that contain a word by putting a minus (hyphen) in front of the word (ex. <code>-opaque</code>) To search for one word or another, use <code>OR</code> (in uppercase) between the words (ex. <code>pizza OR pasta</code>). As an alternative to <code>OR</code>, you can use <code><b>|</b></code> (pipe character). A full query could look something like: <code>restaurant "little italy" pizza OR pasta -mushrooms</code><br/><br/><b>query_fields</b> is used to specify what data in a bundle the query will search. It can contain one or more of <i>insights.spoken_words</i>, metadata fields, and/or bundle fields. Multiple values can be either an array of strings or a comma or space separated single string. By default (if the <b>query_fields</b> param is not included in a request or is a single empty string) all data will be searched.<br/><br/><table><tr><td><b>query_fields</b></td><td><b>Bundle&nbsp;data&nbsp;searched</b></td><td></td></tr><tr><td>*</td><td>all data</td><td>This is the default value.</td></tr><tr><td>insights.spoken_words</td><td><i>[spoken words]</i></td><td>All audio tracks are searched.</td></tr><tr><td><i>fieldname</i></td><td>metadata.<i>fieldname</i></td><td>Your custom metadata field. Wildcard metadata.* searches all metadata fields.</td></tr><tr><td>bundle.<i>fieldname</i></td><td>bundle.<i>fieldname</i></td><td>The searchable bundle fieldnames are name, id, external_id, created and updated. Wildcard bundle.* searches all bundle fields</td></tr></table><br>As an example, suppose you have metadata fields <b>name</b> and <b>description</b> that you would like to search and other metadata fields you don&apos;t want to search. You also want to search the audio words, so you could specify <b>query_fields</b> = &quot;insights.spoken_words, name, description&quot;.<br/><br/><b>filter</b> is used to limit the search results according to specific criteria based on metadata and bundle values. It uses an expression syntax similar to Javascript boolean expressions. An expression is made up of zero or more terms joined by logical operators with each term having a field, a comparison operator, and a literal value. Parentheses can be used to logically group terms.<br/><br/><div class="notes-indent">A filter term is of the form: <b><i><code>field-name comparison-operator literal-value</code></b></i> where:<br/><br/><b><i><code>field-name</code></i></b> is a metadata field or <code>bundle.name</code>, <code>bundle.id</code>, <code>bundle.external_id</code>, <code>bundle.created</code>, or <code>bundle.updated</code>.<br/><br/><b><i><code>comparison-operator</code></i></b> is <code>==</code>, <code>&lt;</code>, <code>&gt;</code>, <code>&lt;=</code>, <code>&gt=</code>, or <code>!=</code><br/><br/><b><i><code>literal-value</code></i></b> is a number (integer or decimal), boolean <code><i>true</i></code> or <code><i>false</i></code>, or a string with either double quotes (<code>"</code>) or single quotes (<code>'</code>).<br/><br/>Logical operators between terms (and groups of terms) can be <code>&&</code> (logical AND), <code>||</code> (logical OR). A logical NOT is <code>!</code> and can be placed before a term (or group of terms.)</div><br/><br/>An example filter expression (assuming you have used metadata fields category and tag): </p><br><div class="notes-indent"><code>category=="music" && (tag == "soft" || tag == "smooth") && tag != "jazz" && bundle.created > "2014-03-15T00:00:00.0Z"</code></div><br/><br/><p><b>language</b> parameter specifies the language of the words in the search query. This value is used for word-stemming etc. while searching text. Regardless of what you set for this parameter, all your bundles will be searched, no matter what language content they contain. Supported languages: en, en-UK, en-US, es, fr.<br/><br/>After getting the initial list, use the <b>first</b>, <b>next</b>, <b>prev</b> link relations to get more bundles in the list. Note that <b>next</b> will not be available at the end of the list and <b>prev</b> will not be available at the start of the list. A maximum of <b>limit</b> items will be returned. If the results are exactly one page neither <b>prev</b> nor <b>next</b> will be available.<br/><br/>The <b>embed</b> parameter specifies link relations to embed in the results. For link relations that are curies (ex. "clarify:metadata"), you may simply use the base name (ex. "metadata").</p>

*Tags:* `search`

#### Input Parameters
* `query` - _optional_ - search terms, typically as typed into a search field. Up to 120 characters.
* `query_fields` - _optional_ - list of insights, metadata, and bundle fields to search with the query. Use insights.spoken_words for searching audio, metadata.* for all metadata fields, bundle.* for all bundle fields, * for audio and all fields. Default is insights.spoken_words and metadata.*. List is space or comma separated single string or an array of strings. If single string, up to 1024 characters.
* `filter` - _optional_ - filter expression, typically programmatically generated based on input controls and data segregation rules etc. Up to 500 characters.
* `language` - _optional_ - Language to search in, specified with an RFC5646 code. Default is "en"
    Possible values: en, en-UK, en-US, es, fr.
* `limit` - _optional_ - limit results to specified number of bundles. Default is 10. Max 100.
* `embed` - _optional_ - list of link relations to embed in the result collection. Zero or more of: items, tracks, metadata, insights. List is space or comma separated single string or an array of strings
* `iterator` - _optional_ - opaque value, automatically provided in next/prev links

## License

**flow**ground :- Telekom iPaaS / clarify-io-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
