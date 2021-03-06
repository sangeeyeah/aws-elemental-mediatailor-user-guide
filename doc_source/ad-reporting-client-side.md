# Client\-side Reporting<a name="ad-reporting-client-side"></a>

With client\-side reporting, MediaTailor proxies the ad tracking URL to the client player\. The player then performs all ad\-tracking activities\. Client\-side reporting enables functionality like trick play for VOD \(players display visual feedback during fast forward and rewind\) and other advanced playback behavior during ad breaks that requires player development \(like no skip\-forward and countdown timers on ad breaks\)\.

Use client\-side reporting for VPAID functionality\. For more information, see [VPAID Handling](vpaid.md)\. The client\-side reporting response includes additional metadata about the VPAID creative\.

**To perform client\-side ad reporting**

1. From the player, initialize a new MediaTailor playback session using a request like the following:

   ```
   POST <mediatailorURL>/v1/session/<hashed-account-ID>/<originID>/<assetID>
       {
           "adsParams": {
              "param1": "value1",
              "param2": "value2",
              "param3": "value3"
          }
       }
   ```

   In the message body JSON: 
   + The `adsParams` are parameter specifications that MediaTailor uses in the request to the ADS\. In the MediaTailor configuration, you define these parameters as `[player_params.param]` in the ADS template URL, as described in [Player Data](variables-player.md)\.
   + Any other query parameters that you provide are forwarded by MediaTailor to your origin server\.

1. AWS Elemental MediaTailor responds to the request with two relative URLs, one for the manifest and one for the tracking endpoint: 
   + Manifest – used to retrieve content manifests and ad segments

     Example:

     ```
     /v1/master/<hashed-account-id>/<originID>/<assetID>?aws.sessionID=<session>
     ```
   + Tracking – used to poll for upcoming ad breaks

     Example: 

     ```
     /v1/tracking/<hashed-account-id>/<originID>/<assetID>/<session>
     ```

   To construct the full manifest and tracking URLs, prefix the relative URLs with *<mediatailorURL>*\. 

1. The player should periodically poll the tracking URL\. When an ad is coming, the response from AWS Elemental MediaTailor to the player's polling request contains a JSON object that specifies the time offsets for the ad breaks\. The offsets are relative to when the player initiated the session\. You can use them when programming specific behaviors in the player, such as preventing the viewer from skipping past the ads\. The response also includes duration, timing, and identification information\. 
   + `adID`: HLS sequence number associated with the beginning of this ad\.
   + `duration`: length in ISO 8601 seconds format\. The response includes durations for the entire ad break and for each ad and beacon \(though beacon durations are always zero\)\. For [VPAID Handling](vpaid.md), the duration conveyed is the MP4 slate duration\. This duration is typically slightly larger than the XML duration conveyed in VAST due to transcoder and segment duration configurations\. You can interpret this as the maximum amount of time that you have to entirely replace with a VPAID ad without incurring drift\.
   + `durationInSeconds`: length in seconds format\. The response includes durations for the entire ad break and for each ad and beacon \(though beacon durations are always zero\)\.
   + `startTime`: time position in ISO 8601 seconds format, relative to the beginning of the playback session\. The response includes start times for the entire ad break and for each ad and beacon\.
   + `startTimeInSeconds`: time position in seconds format, relative to the beginning of the playback session\. The response includes start times for the entire ad break and for each ad and beacon\.
   + `beaconUrls`: where each beacon is sent\.
   + `eventId`: HLS sequence number associated with the beacon\.
   + `eventType`: type of beacon\.
   + `availId`: HLS sequence number associated with the start of the ad break\.
   + `apiFramework`: Set to "`VPAID`"\. Tells the player this is a VPAID ad\.
   + `adParameters`: String of ad parameters from VAST VPAID, which AWS Elemental MediaTailor passes along to the player\.
   + `mediaFilesList`: Assets that the player needs to know about\.
   + `mediaFileUri`: URI that points to either an executable or video asset\. Example: `"https://myad.com/ad/ad134/vpaid.js"`\. 
   + `delivery`: Either "`progressive`" or "`streaming`", depending on the protocol\.
   + `mediaType`: Typically either JavaScript or Flash for executable assets\. 
   + `width`: Width of the video asset\.
   + `height`: Height of the video asset\.
   + `bitrate`: Bit rate of the video asset\. This is not typically included for an executable asset\.
   + `scalable`: Indicates whether to scale the video to other dimensions\.
   + `maintainAspectRatio`: Indicates whether to maintain the aspect ratio while scaling\.
   + `mezzanine`: Specifies a mezzanine MP4 asset, if the VPAID ad includes one\. Example: `"https://gcdn.2mdn.net/videoplayback/id/itag/ck2/file/file.mp4"`\.

   Example responses:

   ```
   {
     "avails": [
       {
         "ads": [
           {
             "adId": "8104385",
             "duration": "PT15.100000078S",
             "durationInSeconds": 15.1,
             "startTime": "PT17.817798612S",
             "startTimeInSeconds": 17.817,
             "trackingEvents": [
   		  {
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=impression"
                 ],
                 "duration": "PT15.100000078S",
                 "durationInSeconds": 15.1,
                 "eventId": "8104385",
                 "eventType": "impression",
                 "startTime": "PT17.817798612S",
                 "startTimeInSeconds": 17.817
               },
               {
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=start"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104385",
                 "eventType": "start",
                 "startTime": "PT17.817798612S",
                 "startTimeInSeconds": 17.817
               },
   			{
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=firstQuartile"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104386",
                 "eventType": "firstQuartile",
                 "startTime": "PT21.592798631S",
                 "startTimeInSeconds": 21.592
               },
   			 {
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=midpoint"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104387",
                 "eventType": "midpoint",
                 "startTime": "PT25.367798651S",
                 "startTimeInSeconds": 25.367
               },
               {
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=thirdQuartile"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104388",
                 "eventType": "thirdQuartile",
                 "startTime": "PT29.14279867S",
                 "startTimeInSeconds": 29.142
               },
               {
                 "beaconUrls": [
                   "http://exampleadserver.com/tracking?event=complete"
                 ],
                 "duration": "PT0S",
                 "durationInSeconds": 0.0,
                 "eventId": "8104390",
                 "eventType": "complete",
                 "startTime": "PT32.91779869S",
                 "startTimeInSeconds": 32.917
               }
             ]
           }
         ],
         "availId": "8104385",
         "duration": "PT15.100000078S",
         "durationInSeconds": 15.1,
         "meta": null,
         "startTime": "PT17.817798612S",
         "startTimeInSeconds": 17.817
       }
     ]
   }
   ```

   ```
   {
     "avails": [
       {
         "ads": [
           {
             "adId": "6744037",
             "mediaFiles": {
               "mezzanine": "https://gcdn.2mdn.net/videoplayback/id/itag/ck2/file/file.mp4",
               "mediaFilesList": [
                 {
                   "mediaFileUri": "https://myad.com/ad/ad134/vpaid.js",
                   "delivery": "progressive",
                   "width": 176,
                   "height": 144,
                   "mediaType": "application/javascript",
                   "scalable": false,
                   "maintainAspectRatio": false,
                   "apiFramework": "VPAID"
                 },
                 {
                   "mediaFileUri": "https://myad.com/ad/ad134/file.mp4",
                   "delivery": "progressive",
                   "width": 640,
                   "height": 360,
                   "mediaType": "video/mp4",
                   "scalable": false,
                   "maintainAspectRatio": false
                 },
                 ...
               ],
               "adParameters": "[{'ads':[{"url":"https://myads/html5/media/LinearVPAIDCreative.mp4","mimetype":"video/mp4"]}]",
               "duration": "PT15.066667079S",
               "durationInSeconds": 15.066,
               "startTime": "PT39.700000165S",
               "startTimeInSeconds": 39.7,
               "trackingEvents": [
                 {
                   "beaconUrls": [
                     "https://beaconURL.com"
                   ],
                   "duration": "PT15.066667079S",
                   "durationInSeconds": 15.066,
                   "eventId": "6744037",
                   "eventType": "impression",
                   "startTime": "PT39.700000165S",
                   "startTimeInSeconds": 39.7
                 },
                 ...
               ]
             }
           },
           ...
         ],
         "availId": "6744037",
         "duration": "PT45.166667157S",
         "durationInSeconds": 45.166,
         "meta": null,
         "startTime": "PT39.700000165S",
         "startTimeInSeconds": 39.7
       }
     ]
   }
   ```