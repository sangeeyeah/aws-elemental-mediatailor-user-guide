# What Is AWS Elemental MediaTailor?<a name="what-is"></a>

AWS Elemental MediaTailor is a scalable ad insertion service that runs in the AWS Cloud\. With MediaTailor, you can serve targeted ads to viewers while maintaining broadcast quality in over\-the\-top \(OTT\) video applications\. 

MediaTailor offers important advances over traditional ad\-tracking systems: ads are better monetized, more consistent in video quality and resolution, and easier to manage across multi\-platform environments\. MediaTailor simplifies your ad workflow by allowing all IP\-connected devices to render ads in the same way as other content\. The service also offers advanced tracking of ad views, which further increases the monetization of content\.

MediaTailor supports Apple HTTP Live Streaming \(HLS\) manifest manipulation\. If you require support for MPEG\-DASH, create a feature request case with [AWS Support](https://console.aws.amazon.com/support/home)\.

**Topics**
+ [Are You a First\-Time User of AWS Elemental MediaTailor?](#are-you-a-first-time-user)
+ [Concepts and Terminology](what-is-terms.md)
+ [How AWS Elemental MediaTailor Works](what-is-flow.md)
+ [Features of AWS Elemental MediaTailor](what-is-features.md)
+ [Related Services](#related-services)
+ [Accessing AWS Elemental MediaTailor](#accessing-emt)
+ [Pricing for AWS Elemental MediaTailor](#pricing)
+ [Regions for AWS Elemental MediaTailor](#regions-endpoints)
+ [Stream Requirements](#stream-reqmts)
+ [Sensitive Information](#sensitive-information)
+ [Transcoded Ad Management](#transcoded-ad-management)

## Are You a First\-Time User of AWS Elemental MediaTailor?<a name="are-you-a-first-time-user"></a>

If you are a first\-time user of AWS Elemental MediaTailor, we recommend that you begin by reading the following sections:
+ [Concepts and Terminology](what-is-terms.md)
+ [How AWS Elemental MediaTailor Works](what-is-flow.md)
+ [Features of AWS Elemental MediaTailor](what-is-features.md)
+ [Getting Started with AWS Elemental MediaTailor](getting-started.md)

## Related Services<a name="related-services"></a>
+ **Amazon CloudFront** is a global content delivery network \(CDN\) service that securely delivers data and videos to your viewers\. Use CloudFront to deliver content with the best possible performance\. For more information about CloudFront, see the [Amazon CloudFront website](https://aws.amazon.com/cloudfront/)\.
+ **AWS Elemental MediaPackage** is a just\-in\-time packaging and origination service that customizes live video assets for distribution in a format that is compatible with the device that makes the request\. Use AWS Elemental MediaPackage as an origin server to prepare content and add ad markers before sending streams to MediaTailor\. For more information about how MediaTailor works with origin servers, see [How AWS Elemental MediaTailor Works](what-is-flow.md)\.
+ **AWS Identity and Access Management \(IAM\)** is a web service that helps you securely control access to AWS resources for your users\. Use IAM to control who can use your AWS resources \(authentication\) and what resources they can use in which ways \(authorization\)\. For more information, see [Setting Up AWS Elemental MediaTailor](setting-up.md)\.

## Accessing AWS Elemental MediaTailor<a name="accessing-emt"></a>

You can access MediaTailor using the service's console\.

You must access your AWS account by providing credentials that verify that you have permissions to use the services\. 

To log in to the MediaTailor console, use the following link: **https://console\.aws\.amazon\.com/mediatailor/home**\.

## Pricing for AWS Elemental MediaTailor<a name="pricing"></a>

As with other AWS products, there are no contracts or minimum commitments for using MediaTailor\. You are charged based your use of the service\. For more information, see [MediaTailor Pricing](https://aws.amazon.com/mediatailor/pricing/)\.

## Regions for AWS Elemental MediaTailor<a name="regions-endpoints"></a>

To reduce data latency in your applications, MediaTailor offers regional endpoints to make your requests\. To view the list of regions in which MediaTailor is available, see [https://docs.aws.amazon.com/general/latest/gr/rande.html#mediatailor_region](https://docs.aws.amazon.com/general/latest/gr/rande.html#mediatailor_region)\.

## Stream Requirements<a name="stream-reqmts"></a>

A video stream must meet the following requirements to work with AWS Elemental MediaTailor:
+ Uses HLS \(Apple HTTP Live Streaming\)
+ Uses live streaming or video\-on\-demand \(VOD\)
+ Is accessible on the public internet and has a public IP address
+ Contains ad markers in one of the formats described in [Step 2: Prepare a Stream](getting-started.md#getting-started-prep-stream)

## Sensitive Information<a name="sensitive-information"></a>

AWS Elemental MediaTailor does not require that you supply any customer data\.

Do not put sensitive information, like customer account numbers, credit card information, or passwords, into free\-form fields or query parameters\. This applies to all use of AWS Elemental MediaTailor, including the console, API, SDKs, and the AWS CLI\. Any data that you enter into the service might get picked up for inclusion in diagnostic logs\.

When you provide a URL to an external server, do not include unencrypted credentials information in the URL to validate your request to that server\.

## Transcoded Ad Management<a name="transcoded-ad-management"></a>

AWS Elemental MediaTailor manages transcoded ads on your behalf with no additional charge\. When you play ads in a video stream, depending on where your ad data is located, it might get copied to another AWS Region\.

 If you need to delete your transcoded ad assets for any reason, file a basic support ticket through the AWS Support console with the category of "service limit increase\."