---
description: Adobe Target integrates with your web pages by means of the at.js or mbox.js JavaScript library.
keywords: targeting;cookie;first-party cookie;1st-party cookie
seo-description: Adobe Target integrates with your web pages by means of the at.js or mbox.js JavaScript library.
seo-title: How Targeting Works
solution: Target
title: How Targeting Works
uuid: 81dbc038-4f11-431c-929e-3a8f9fd9c0da
index: y
internal: n
snippet: y
translate: y
---

# How Targeting Works

[!DNL  Target Classic] used mboxes around each area on your page where you want to display targeted content or collect data. These mboxes are not required in [!DNL  Target Standard]. Instead, one [ JavaScript library](../c_seting_up_target/c_implementing_target/c_target-implement.md#concept_60B748DE4293488F917E8F1FA4C7E9EB) referenced on each page is all you need to run your optimization activities. 

Each time a visitor requests a Target-enabled page, [!DNL  Target] uses the following process to serve offers: 


1. A customer requests a page from your server and it displays in the browser. 

1. A first-party cookie is set in the customer's browser to set a unique visitor ID. 

   A [!DNL  Experience Cloud visitor ID] is also set if you are using the Master Marketing Profile. 

1. The page makes a call to [!DNL  Target] either via the [!DNL  target.js] file or an mbox on your page. 

1. [!DNL  Target] references the profile associated with the visitor. 

1. [!DNL  Target] executes any profile scripts associated with that profile. 

1. [!DNL  Target] calculates its response. 

1. Content is displayed based on the rules of your activity or campaign. 



The offer that is displayed in a basic A/B test is randomly chosen. As a result of this random splitting of traffic, it can take a lot of initial traffic before the percentages even out. For example, if you have an activity with two experiences, the starting experience is chosen randomly. If there is little traffic, it's possible that the percentage of visitors can be skewed toward one experience. As traffic increases, the percentages should become more equal. 