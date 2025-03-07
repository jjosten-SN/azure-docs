---
title: What is Azure Video Indexer?
description: This article gives an overview of the Azure Video Indexer service.
ms.topic: overview
ms.date: 06/09/2022
ms.author: juliako
---

# What is Azure Video Indexer?

[!INCLUDE [regulation](./includes/regulation.md)]

> [!NOTE]
> The service is now rebranded from Azure Video Analyzer for Media to **Azure Video Indexer**. Click [here](https://vi.microsoft.com) to read more.

Azure Video Indexer is a cloud application, part of Azure Applied AI Services, built on Azure Media Services and Azure Cognitive Services (such as the Face, Translator, Computer Vision, and Speech). It enables you to extract the insights from your videos using Azure Video Indexer video and audio models.

Azure Video Indexer analyzes the video and audio content by running 30+ AI models, generating rich insights. Below is an illustration of the audio and video analysis performed by Azure Video Indexer in the background.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/video-indexer-overview/model-chart.png" alt-text="Diagram of Azure Video Indexer flow.":::

To start extracting insights with Azure Video Indexer, you need to [create an account](connect-to-azure.md) and upload videos, see the [how can i get started](#how-can-i-get-started-with-azure-video-indexer) section below.

## Compliance, Privacy and Security

As an important reminder, you must comply with all applicable laws in your use of Azure Video Indexer, and you may not use Azure Video Indexer or any Azure service in a manner that violates the rights of others, or that may be harmful to others.

Before uploading any video/image to Azure Video Indexer, You must have all the proper rights to use the video/image, including, where required by law, all the necessary consents from individuals (if any) in the video/image, for the use, processing, and storage of their data in Azure Video Indexer and Azure. Some jurisdictions may impose special legal requirements for the collection, online processing and storage of certain categories of data, such as biometric data. Before using Azure Video Indexer and Azure for the processing and storage of any data subject to special legal requirements, You must ensure compliance with any such legal requirements that may apply to You.

To learn about compliance, privacy and security in Azure Video Indexer please visit the Microsoft [Trust Center](https://www.microsoft.com/TrustCenter/CloudServices/Azure/default.aspx). For Microsoft's privacy obligations, data handling and retention practices, including how to delete your data, please review Microsoft's [Privacy Statement](https://privacy.microsoft.com/PrivacyStatement), the [Online Services Terms](https://www.microsoft.com/licensing/product-licensing/products?rtc=1) ("OST") and [Data Processing Addendum](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA"). By using Azure Video Indexer, you agree to be bound by the OST, DPA and the Privacy Statement.

## What can I do with Azure Video Indexer?

Azure Video Indexer's insights can be applied to many scenarios, among them are:

* *Deep search*: Use the insights extracted from the video to enhance the search experience across a video library. For example, indexing spoken words and faces can enable the search experience of finding moments in a video where a person spoke certain words or when two people were seen together. Search based on such insights from videos is applicable to news agencies, educational institutes, broadcasters, entertainment content owners, enterprise LOB apps, and in general to any industry that has a video library that users need to search against.
* *Content creation*: Create trailers, highlight reels, social media content, or news clips based on the insights Azure Video Indexer extracts from your content. Keyframes, scenes markers, and timestamps of the people and label appearances make the creation process smoother and easier, enabling you to easily get to the parts of the video you need when creating content.
* *Accessibility*: Whether you want to make your content available for people with disabilities or if you want your content to be distributed to different regions using different languages, you can use the transcription and  translation provided by Azure Video Indexer in multiple languages.
* *Monetization*: Azure Video Indexer can help increase the value of videos. For example, industries that rely on ad revenue (news media, social media, and so on) can deliver relevant ads by using the extracted insights as additional signals to the ad server.
* *Content moderation*: Use textual and visual content moderation models to keep your users safe from inappropriate content and validate that the content you publish matches your organization's values. You can automatically block certain videos or alert your users about the content.
* *Recommendations*: Video insights can be used to improve user engagement by highlighting the relevant video moments to users. By tagging each video with additional metadata, you can recommend to users the most relevant videos and highlight the parts of the video that will match their needs.

## Features

The following list shows the insights you can retrieve from your videos using Azure Video Indexer video and audio models:

### Video insights

* **Face detection**: Detects and groups faces appearing in the video.
* **Celebrity identification**: Azure Video Indexer automatically identifies over 1 million celebrities—like world leaders, actors, actresses, athletes, researchers, business, and tech leaders across the globe. The data about these celebrities can also be found on various websites (IMDB, Wikipedia, and so on).
* **Account-based face identification**: Azure Video Indexer trains a model for a specific account. It then recognizes faces in the video based on the trained model. For more information, see [Customize a Person model from the Azure Video Indexer website](customize-person-model-with-website.md) and [Customize a Person model with the Azure Video Indexer API](customize-person-model-with-api.md).
* **Thumbnail extraction for faces** ("best face"): Automatically identifies the best captured face in each group of faces (based on quality, size, and frontal position) and extracts it as an image asset.
* **Visual text recognition** (OCR): Extracts text that's visually displayed in the video.
* **Visual content moderation**: Detects adult and/or racy visuals.
* **Labels identification**: Identifies visual objects and actions displayed.
* **Scene segmentation**: Determines when a scene changes in video based on visual cues. A scene depicts a single event and it's composed by a series of consecutive shots, which are semantically related.
* **Shot detection**: Determines when a shot changes in video based on visual cues. A shot is a series of frames taken from the same motion-picture camera. For more information, see [Scenes, shots, and keyframes](scenes-shots-keyframes.md).
* **Black frame detection**: Identifies black frames presented in the video.
* **Keyframe extraction**: Detects stable keyframes in a video.
* **Rolling credits**: Identifies the beginning and end of the rolling credits in the end of TV shows and movies.
* **Animated characters detection** (preview): Detection, grouping, and recognition of characters in animated content via integration with [Cognitive Services custom vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). For more information, see [Animated character detection](animated-characters-recognition.md).
* **Editorial shot type detection**: Tagging shots based on their type (like wide shot, medium shot, close up, extreme close up, two shot, multiple people, outdoor and indoor, and so on). For more information, see [Editorial shot type detection](scenes-shots-keyframes.md#editorial-shot-type-detection).
* **Observed People Tracking** (preview): detects observed people in videos and provides information such as the location of the person in the video frame (using bounding boxes) and the exact timestamp (start, end) and confidence when a person appears. For more information, see [Trace observed people in a video](observed-people-tracing.md).
    * **People's detected clothing**: detects the clothing types of people appearing in the video and provides information such as long or short sleeves, long or short pants and skirt or dress. The detected clothing is associated with the people wearing it and the exact timestamp (start,end) along with a confidence level for the detection are provided.
* **Matched person**: matches between people that were observed in the video with the corresponding faces detected. The matching between the observed people and the faces contain a confidence level.

### Audio insights

* **Audio transcription**: Converts speech to text over 50 languages and allows extensions. Supported languages include English US, English United Kingdom, English Australia, Spanish, Spanish(Mexico), French, French(Canada), German, Italian, Mandarin Chinese, Chinese (Cantonese, Traditional), Chinese (Simplified), Japanese, Russian, Portuguese, Hindi, Czech, Dutch, Polish, Danish, Norwegian, Finish, Swedish, Thai, Turkish, Korean, Arabic(Egypt), Arabic(Syrian Arab Republic), Arabic(Israel), Arabic(Iraq), Arabic(Jordan), Arabic(Kuwait), Arabic(Lebanon), Arabic(Oman), Arabic(Qatar), Arabic(Saudi Arabia), Arabic(United Arab Emirates), Arabic(Palestinian Authority) and Arabic Modern Standard (Bahrain) .
* **Automatic language detection**: Automatically identifies the dominant spoken language. Supported languages include English, Spanish, French, German, Italian, Mandarin Chinese, Japanese, Russian, and Portuguese. If the language can't be identified with confidence, Azure Video Indexer assumes the spoken language is English. For more information, see [Language identification model](language-identification-model.md).
* **Multi-language speech identification and transcription**: Automatically identifies the spoken language in different segments from audio. It sends each segment of the media file to be transcribed and then combines the transcription back to one unified transcription. For more information, see [Automatically identify and transcribe multi-language content](multi-language-identification-transcription.md).
* **Closed captioning**: Creates closed captioning in three formats: VTT, TTML, SRT.
* **Two channel processing**: Auto detects separate transcript and merges to single timeline.
* **Noise reduction**: Clears up telephony audio or noisy recordings (based on Skype filters).
* **Transcript customization** (CRIS): Trains custom speech to text models to create industry-specific transcripts. For more information, see [Customize a Language model from the Azure Video Indexer website](customize-language-model-with-website.md) and [Customize a Language model with the Azure Video Indexer APIs](customize-language-model-with-api.md).
* **Speaker enumeration**: Maps and understands which speaker spoke which words and when. Sixteen speakers can be detected in a single audio-file.
* **Speaker statistics**: Provides statistics for speakers' speech ratios.
* **Textual content moderation**: Detects explicit text in the audio transcript.
* **Emotion detection**: Identifies emotions based on speech (what's being said) and voice tonality (how it's being said). The emotion could be joy, sadness, anger, or fear.
* **Translation**: Creates translations of the audio transcript to 54 different languages.
* **Audio effects detection** (preview): Detects the following audio effects in the non-speech segments of the content: alarm or siren, dog barking, crowd reactions (cheering, clapping, and booing), gunshot or explosion, laughter, breaking glass, and silence.

    The detected acoustic events are in the closed captions file. The file can be downloaded from the Azure Video Indexer portal. For more information, see [Audio effects detection](audio-effects-detection.md).

    > [!NOTE]
    > The full set of events is available only when you choose **Advanced Audio Analysis** when uploading a file, in upload preset. By default, only silence is detected.

### Audio and video insights (multi-channels)

When indexing by one channel, partial result for those models will be available.

* **Keywords extraction**: Extracts keywords from speech and visual text.
* **Named entities extraction**: Extracts brands, locations, and people from speech and visual text via natural language processing (NLP).
* **Topic inference**: Extracts topics based on various keywords (i.e. keywords 'Stock Exchange', 'Wall Street' will produce the topic 'Economics'). The model uses three different ontologies ([IPTC](https://iptc.org/standards/media-topics/), [Wikipedia](https://www.wikipedia.org/) and the Video Indexer hierarchical topic ontology). The model uses transcription (spoken words), OCR content (visual text), and celebrities recognized in the video using the Video Indexer facial recognition model. 
* **Artifacts**: Extracts rich set of "next level of details" artifacts for each of the models.
* **Sentiment analysis**: Identifies positive, negative, and neutral sentiments from speech and visual text.

## How can I get started with Azure Video Indexer?

### Prerequisite

Before creating a new account, review [Account types](accounts-overview.md).

### Start using Azure Video Indexer

You can access Azure Video Indexer capabilities in three ways:

* Azure Video Indexer portal: An easy-to-use solution that lets you evaluate the product, manage the account, and customize models.

    For more information about the portal, see [Get started with the Azure Video Indexer website](video-indexer-get-started.md).  

* API integration: All of Azure Video Indexer's capabilities are available through a REST API, which lets you integrate the solution into your apps and infrastructure.

    To get started as a developer, see [Use Azure Video Indexer REST API](video-indexer-use-apis.md).

* Embeddable widget: Lets you embed the Azure Video Indexer insights, player, and editor experiences into your app.

    For more information, see [Embed visual widgets in your application](video-indexer-embed-widgets.md).

If you're using the website, the insights are added as metadata and are visible in the portal. If you're using APIs, the insights are available as a JSON file.

## Supported browsers

The following list shows the supported browsers that you can use for the Azure Video Indexer website and for your apps that embed the widgets. The list also shows the minimum supported browser version:

- Edge, version: 16
- Firefox, version: 54
- Chrome, version: 58
- Safari, version: 11
- Opera, version: 44
- Opera Mobile, version: 59
- Android Browser, version: 81
- Samsung Browser, version: 7
- Chrome for Android, version: 87
- Firefox for Android, version: 83

## Next steps

You're ready to get started with Azure Video Indexer. For more information, see the following articles:

- [Pricing](https://azure.microsoft.com/pricing/details/video-indexer/)
- [Get started with the Azure Video Indexer website](video-indexer-get-started.md).
- [Process content with Azure Video Indexer REST API](video-indexer-use-apis.md).
- [Embed visual widgets in your application](video-indexer-embed-widgets.md).
