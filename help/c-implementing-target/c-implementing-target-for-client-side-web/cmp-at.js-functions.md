---
description: List of functions that can be used with at.js.
keywords: adobe.target.notification;element;selector;notification;extension
seo-description: List of functions that can be used with the at.js JavaScript library in Adobe Target.
seo-title: Adobe Target at.js functions
solution: Target
subtopic: Getting Started
title: at.js functions
topic: Standard
uuid: ec5f27a7-b22a-48c9-968c-9eb02830a2a6
mini-toc-levels: 1 
---

# at.js functions{#at-js-functions}

List of functions that can be used with the Adobe Target at.js JavaScript library.

## adobe.target.getOffer(options) {#reference_C81525D1598A4A1199740DCAB81A7FDF}

This function fires a request to get a Target offer.

Use with `adobe.target.applyOffer()` to process the response or use your own success handling. The options parameter is mandatory and has the following structure:

| Key | Type | Required | Description |
|--- |--- |--- |--- |
|mbox|String|Yes|Mbox name|
|params|Object|No|Mbox parameters. An object of key-value pairs that has the following structure:<br>`{ "param1": "value1", "param2": "value2"}`|
|success|Function|Yes|Callback to be executed when we got a response from the server. The success callback function will receive a single parameter that represents an array of offer objects. Here is a success callback example:<br>`function handleSuccess(response){......}`<br>See Responses below for details.|
|error|Function|Yes|Callback to be executed when we got an error. There are a few cases that are considered erroneous:<ul><li>HTTP status code different from 200 OK</li><li>Response can not be parsed. For example we poorly constructed JSON or HTML instead of JSON.</li><li>Response contains the "error" key. For example an exception was thrown on the edge a request could not be properly processed. We could get an error when an mbox is blocked and we could not retrieve any content for it, etc. The error callback function will receive two parameters: status and error. Here is an error callback example: `function handleError(status, error){......}`</li></ul>See Error Responses below for details.|
|timeout|Number|No|Timeout in milliseconds. If not specified, the default timeout in at.js will be used.<br>The default timeout can be set from the [!DNL Target] UI under [!UICONTROL Setup > Implementation > Edit Mbox.js Settings > Timeout].|

### Examples {#section_97C2D2E03E6549BEA7F4873E3F5E4A0D}

Adding parameters with getOffer() and using applyOffer() for success-handling:

```
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

Adding parameters and profile parameters with getOffer() and using applyOffer() for success-handling:

```
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2, 
     "profile.age": 27, 
     "profile.gender": "male" 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

Using custom timeout and custom success-handling with getOffer():

"YOUR_OWN_CUSTOM_HANDLING_FUNCTION" is a placeholder for a function the customer would define.

```
adobe.target.getOffer({     
  "mbox": "target-global-mbox",   
  "success": function(offer) { 
    YOUR_OWN_CUSTOM_HANDLING_FUNCTION(offer);   
  }, 
  "error": function(status, error) {                 
    console.log('Error', status, error);   
  },   
  "timeout": 2000 
});
```

### Responses {#section_CF9FD236EF794620BCBF84EB80160183}

The response parameter passed to the success callback will be an array of actions. An action is an object that usually has the following format:

| Name | Type | Description |
|--- |--- |--- |
|action|String|Type of action to be applied to the identified element.|
|selector|Sting|Represents a Sizzle selector.|
|cssSelector|String|DOM native selector, used for element pre-hiding.|
|content|String|The content to be applied to the identified element.|

### Example

```
{ 
    "sessionId": "1444512212156-384616", 
    "tntId": "1444512212156-384616.17_35", 
    "offers": [{ 
        "plugins": ["<script type=\"text/javascript\">\r\n/*mboxHighlight+ (1of2) v1 ==> Response Plugin*/\r\nwindow.ttMETA=(typeof(window.ttMETA)!='undefined')?window.ttMETA:[];window.ttMETA.push({'mbox':'target-global-mbox','campaign':'at: redirect ootb','experience':'Experience B','offer':'/at_redirect_ootb/experiences/1/pages/0/1442082890250'});window.ttMBX=function(x){var mbxList=[];for(i=0;i<ttMETA.length;i++){if(ttMETA[i].mbox==x.getName()){mbxList.push(ttMETA[i])}}return mbxList[x.getId()]}\r\n</script>"], 
        "actions": { 
            "content": [{ 
                "passMboxSession": false, 
                "selector": "body", 
                "action": "redirect", 
                "url": "https://example.com/04.html", 
                "includeAllUrlParameters": true 
            }] 
        } 
    }] 
}
```

### Error Responses {#section_1ACCE79AF2CB4FA2AD1371EA06AF129F}

The "status" and "error" parameters passed to the error callback will have the following format:

| Name | Type | Description |
|--- |--- |--- |
|status|String|Represents the error status. This parameter can have the following values:<ul><li>timeout: Indicates that the request timed out.</li><li>parseerror: Indicates that the response could not be parsed, for example if we receive HTML or plain text instead of JSON.</li><li>error: Indicates a general error like we received HTTP status different from 200 OK</li></ul>|
|error|String|Contains additional data like exception message or anything else that might be useful for troubleshooting.|

## adobe.target.getOffers(options) - at.js 2.0.0 {#adobe-target-getOffers}

This function lets you retrieve multiple offers by passing in multiple mboxes. Additionally, multiple offers can be retrieved for all views in active activities.

>[!NOTE]
>
>This function was introduced with at.js 2.0. This function is not available for at.js version 1.*x*.

|Key|Type|Required?|Description|
| --- | --- | --- | --- |
|consumerId|String|No|Default value is client's global mbox if not provided. This key is used to generate the supplemental data ID used for A4T integration.|
|request<br>|Object|Yes|See Requests table below.|
|timeout|Number|No|request timeout. If not specified the default at.js timeout is used.|

### Request

|Field name|Required?|Limitations|Description|
| --- | --- | ---| --- |
|request > id|No||One of `tntId`, `thirdPartyId`, or `marketingCloudVisitorId` is required.|
|Request > id > thirdPartyId|No|Maximum size = 128||
|Request > prefetch|No|||
|Request > prefetch > views|No|Maximim count 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=` 5000<br>Name should not start with "profile"<br>Not allowed names: "orderId", "orderTotal", "productPurchasedId"|Pass in parameters to be used to retrieve relevant views in active activities.|
|Request > prefetch > views > profileParameters|No|Maximim count 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=` 5000<br>Name should not start with "profile"|Pass in profile parameters to be used to retrieve relevant views in active activities.|
|Request > prefetch > views > product|No|||
|Request > prefetch > views > product -> id|No|Not blank<br>maximum size = 128|Pass in product IDs to be used to retrieve relevant views in active activities.|
|Request > prefetch > views > product > categoryId|No|Not blank<br>maximum size = 128|Pass in product category IDs to be used to retrieve relevant views in activities.|
|Request > prefetch > views > order|No|||
|Request > prefetch > views > order > id|No|Maximum length = 250|Pass in order IDs to be used to retrieve relevant views in in active activities.|
|Request > prefetch > views > order > total|No|Total `>=` 0|Pass in order totals to be used to retrieve relevant views in active activities.|
|Request > prefetch > views > order > purchasedProductIds|No|No blank values<br>Each value's max length 50<br>Concatenated and separated by comma<br>Product IDs total length `<=` 250|Pass in purchased product IDs to be used to retrieve relevant views in active activities.|
|Request > execute|No|||
|Request > execute > pageLoad|No|||
|Request > execute > pageLoad > parameters|No|Maximum count 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=` 5000<br>Name should not start with "profile."<br>Not allowed names: "orderId", "orderTotal", "productPurchasedId"|Retrieve offers with specified parameters when page loads.|
|Request > execute > pageLoad > profileParameters|No|Maximum count 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=`256<br>Name should not start with "profile."|Retrieve offers with specified profile parameters when page loads.|
|Request > execute > pageLoad > product|No|||
|Request > execute > pageLoad > product -> id|No|Not blank<br>Maximum size = 128|Retrieve offers with specified product IDs when page loads.|
|Request > execute > pageLoad > product > categoryId|No|Not blank<br>Maximum size = 128|Retrieve offers with specified product category IDs when page loads.|
|Request > execute > pageLoad > order|No|||
|Request > execute > pageLoad > order > id|No|Maximum length = 250|Retrieve offers with specified order IDs when page loads.|
|Request > execute > pageLoad > order > total|No|`>=` 0|Retrieve offers with specified order totals when page loads.|
|Request > execute > pageLoad > order > purchasedProductIds|No|No blank values<br>Each value's max length 50<br>Concatenated and separated by comma<br>Product IDs total length `<=` 250|Retrieve offers with specified purchased product IDs when page loads.|
|Request > execute > mboxes|No|Maximum size = 50<br>No null elements||
|Request > execute > mboxes>mbox|Yes|Not blank<br>No '-clicked' suffix<br>Maximum size = 250<br>Allowed characters: `'-, ._\/=:;&!@#$%^&*()_+|?~[]{}'`|Name of mbox.|
|Request > execute > mboxes>mbox>index|Yes|Not null<br>Unique<br>`>=` 0|Note that the index does not represent the order in which the mboxes will be processed. Same as in a web page with several regional mboxes, the order in which they will be processed cannot be specified.|
|Request > execute > mboxes > mbox > parameters|No|Maximum count = 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=` 5000<br>Name should not start with "profile."<br>Not allowed names: "orderId", "orderTotal", "productPurchasedId"|Retrieve offers for a given mbox with the specified parameters.|
|Request > execute > mboxes>mbox>profileParameters|No|Maximum count = 50<br>Name not blank<br>Name length `<=` 128<br>Value length `<=`256<br>Name should not start with "profile."|Retrieve offers for a given mbox with the specified profile parameters.|
|Request > execute > mboxes>mbox > product|No|||
|Request > execute > mboxes > mbox > product > id|No|Not blank<br>Maximum size = 128|Retrieve offers for a given mbox with the specified product IDs.|
|Request > execute > mboxes > mbox > product > categoryId|No|Not blank<br>Maximum size = 128|Retrieve offers for a given mbox with the specified product category IDs.|
|Request > execute > mboxes > mbox > order|No|||
|Request > execute > mboxes>mbox > order > id|No|Maximum length = 250|Retrieve offers for a given mbox with the specified order IDs.|
|Request > execute > mboxes > mbox > order > total|No|`>=` 0|Retrieve offers for a given mbox with the specified order totals.|
|Request > execute > mboxes > mbox > order > purchasedProductIds|No|No blank values<br>Each value's maximum length = 50<br>Concatenated and separated by comma<br>Product ids total length `<=` 250|Retrieve offers for a given mbox with the specified order purchased product IDs.|

### Call `getOffers()` for all views

```
adobe.target.getOffers({
    prefetch: {
      views: []
    }
  }
});
```

### Call `getOffers()` to retrieve the latest views with the passed-in parameters and profile parameters

```
adobe.target.getOffers({
  request: {
    "prefetch": {
      "views": [
        {
          "parameters": {
            "ad": "1"
          },
          "profileParameters": {
            "age": 23
          }
        }
      ]
    }
  }
});
```

### Call `getOffers()` to retrieve mboxes with parameters and profile parameters passed-in.

```
adobe.target.getOffers({
  request: {
    execute: {
      mboxes: [
        {
          index: 0,
          name: "first-mbox"
        },
        {
          index: 1,
          name: "second-mbox",
          parameters: {
            a: 1
          },
          profileParameters: {
            b: 2
          }
        }
      ]
    }
  }
});
```

## adobe.target.applyOffer(options) {#reference_BBE83F513B5B4E03BBC3F50D90864245}

This function is for applying the response content.

>[!NOTE]
>
>`applyOffer` requires the `mbox` parameter. If no mbox name is specified, an error occurs.

The options parameter is mandatory and has the following structure:

| Key | Type | Required | Description |
|--- |--- |--- |--- |
|mbox|String|Yes|Mbox name<br>With at.js 1.3.0 (and later) Target enforces that the mbox key is used. This key has been required in the past, but Target now enforces its use to ensure that Target has proper validation and customers are using the function correctly.|
|selector|String or DOM Element|No|HTML element or CSS selector used to identify the HTML element where Target should place the offer content. If selector is not provided, Target assumes that the HTML element we should use is HTML HEAD.|
|offer|Array|Yes|An array actions that should be applied to the element.|

### Example {#section_D8D6A17B73DE4542937CDB687193A5CC}

The following example shows how to use `getOffer` and `applyOffer` together:

```
adobe.target.getOffer({   
  "mbox": "mbox",   
  "success": function(offers) {           
        adobe.target.applyOffer( {  
           "mbox": "mbox", 
           "offer": offers  
        } ); 
  },   
  "error": function(status, error) {           
      if (console && console.log) { 
        console.log(status); 
        console.log(error); 
      } 
  }, 
 "timeout": 5000 
}); 
```

## adobe.target.applyOffers(options) - at.js 2.0.0 {#adobe-target-applyOffers}

This function lets you apply more than one offer that was retrieved by `adobe.target.getOffers()`.

>[!NOTE]
>
>This function was introduced with at.js 2.0. This function is not available for at.js version 1.*x*.

|Key|Type|Required?|Description|
| --- | --- | --- | --- |
|selector|String|No|HTML element or CSS selector used to identify the HTML element where [!DNL Target] should place the offer content. If a selector is not provided, [!DNL Target] assumes that the HTML element to should use is HTML HEAD.|
|Response|Object|Yes|Response object from `getOffers()`.<br>See Requests table below.|

### Request

|Field Name|Description|
| --- | --- |
|response > prefetch > views > options > content|Note that the content of the "option" is not well-defined and depends directly on the option type/template structure.|
|response > prefetch > views > options > type|Option type. Reflects type of "content" field. Supported type is actions.|
|response > prefetch > views > state|An opaque view state token that should be forwarded with display notification for the view|
|response > prefetch  > views > options > responseTokens|Contains the map of `responseTokens` that have been collected when the current option was being processed.|
|response > prefetch  > views > analytics > payload|Analytics payload for client-side integration that should be sent to Analytics after the view has been applied.|
|response > prefetch  > views > trace|The object containing all trace data for the prefetch call per view.<br>The trace object will also include a version for the trace.<br>The trace object will also include details of the current view.|
|response > prefetch  > views > options > eventToken|Event logging is done per option. For every applied option the respective event token should to be added to the list of notification tokens. Note that a View is composed of multiple options. If all options have been applied and seen, all `eventTokens` need to be included in the notification.|
|response > prefetch  > views > name|The human-readable view name.|
|response > prefetch  > views > metrics|Reporting metrics that should be watched and then notify [!DNL Target] about. Currently, just click metric is supported. In case a click on the element happens, the appropriate `eventTokens` should be collected and a notification should be sent.|
|response > prefetch  > views > key|The key or fingerprint that identifies the view.|
|response > prefetch  > views > id|ID of the view.|
|response > notifications > id|Notification ID.|
|response > notifications > events > type|The type of the notification, click, or display.|
|response > notifications > events > trace|The trace for the notification event.|
|response > notifications > events > token|The token that was sent with the notification event.|
|response > notifications > events > timestamp|The timestamp that was sent with the notification event.|
|response > notifications > events > errorCode|If the notification failed, the code indicates the reason for the failure.|
|response > notifications > events|The events that were logged or failed to be logged for the current notification.|
|response > notifications|Indicates the logged or failed notifications.|
|response > execute > mboxes > mbox > trace|The object containing all trace data for the individual mbox request.|
|response > execute > mboxes > mbox > responseTokens|Contains map of `responseTokens` for specific mbox request execution.|
|response > execute > mboxes > mbox > option > content|Note that the content of the "option" is not well-defined and depends directly on the option type/template structure.|
|response > execute > mboxes > mbox > option > type|Option type. Reflects type of "content" field. Supported types are: html, redirect, JSON, and dynamic.|
|response > execute > mboxes > mbox > options|Response option.|
|response > execute > mboxes > mbox > metrics > eventToken|Token of click event.|
|response > execute > mboxes > mbox > metrics > type|"click"|
|response > execute > mboxes > mbox > metrics|Contains list of `clickThrough` metrics.|
|response > execute > mboxes > mbox > mbox|The name of the mbox.|
|response > execute > mboxes > mbox >index|Indicates that the response is for mbox with this index from the request.|
|response > execute > mboxes > mbox > analytics > payload|Analytics payload for client-side integration that should be sent to Analytics after the mbox has been applied. (See A4T-enabled Campaigns section.)|
|response > execute > mboxes|List of executed mboxes.|
|response > execute > pageLoad > options > content|Note that the content of the "option" is not well-defined and depends directly on the option type/template structure.|
|response > execute > pageLoad > options > type|Option type. Reflects type of "content" field. Supported types are: html, redirect, JSON, dynamic, and actions.|
|response > execute > pageLoad > options|Options that aren't grouped by views (target-global-mbox + options from activities with views not grouped by views).|
|response > execute > pageLoad > metrics|Click metrics that were not set to belong to a specific view.|
|response > execute > pageLoad > trace|The object containing all trace data for the pageLoad request.|
|response > execute > pageLoad > analytics > payload|Analytics payload for client-side integration that should be sent to Analytics after the page load content has been applied. (See A4T-enabled Campaigns section.)|

### Example applyOffers() call

```
adobe.target.applyOffers({response:{
  "execute": {
    "pageLoad": {
      "options": [{
        "type": "html",
        "content": "page-load"
      },
      {
        "type": "actions",
        "content": [{
          "type": "setHtml",
          "content": "<h1>Container 1</h1>",
          "selector": "#container1",
          "cssSelector": "#container1"
        },
        {
          "type": "setHtml",
          "content": "<h3>Container 3</h3>",
          "selector": "#container3",
          "cssSelector": "#container3"
        }]
      }],
 
 
      "metrics": [{
        "type": "click",
        "selector": "#container1",
        "eventToken": "page-load-click-metric" 
      }]
    }
  }
}});
```

### Example call of Promise chaining with `getOffers()` and `applyOffers()`, because these functions are Promise based

```
adobe.target.getOffers({...})
.then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

## adobe.target.triggerView (viewName, options) - at.js 2.0.0 {#adobe-target-triggerView}

This function can be called whenever a new page is loaded or when a component on a page is re-rendered. `adobe.target.triggerView()` should be implemented for single page applications (SPAs) in order to use the Visual Experience Composer (VEC) to create A/B Tests and Experience Targeting (XT) activities. If `adobe.target.triggerView()` is not implemented on the site, the VEC cannot be utilized for SPA. For more information, see [Single Page Application implementation](/help/c-implementing-target/c-implementing-target-for-client-side-web/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>This function was introduced with at.js 2.0. This function is not available for at.js version 1.*x*.

|Parameter|Type|Required?|Description|
| --- | --- | --- | --- |
|viewName|String|Yes|Pass in any name as a string type that you want to represent your view. This view name appears in the [!UICONTROL Modifications] panel of the VEC for marketers to create actions and run their A/B and XT activities.|
|options|Object|No||
|options > page|Boolean|No|**TRUE:** Default value of page is true. When page=true, notifications are sent to the [!DNL Target] backend for incrementing impression count.<br>**FALSE:** When page=false, notifications are sent for incrementing impression count. This should be used when you want to only re-render a component on a page with an offer.|

### Example `triggerView()` call that will send a notification to the Target backend for incrementing impression count

```
adobe.target.triggerView("homeView")
```

### Example `triggerView()` call to not have notifications sent to the Target backend for impression counting

```
adobe.target.triggerView("homeView", {page: false})
```

## adobe.target.trackEvent(options) {#reference_7E0F19368F9C4BC38F1E5DC5E717E487}

This function fires a request to report user actions, such as clicks and conversions. It does not deliver activities in the response.

These event-tracking mbox calls can then be used to define metrics in activities. For more information, see [Success Metrics](../../c-activities/r-success-metrics/success-metrics.md#reference_D011575C85DA48E989A244593D9B9924) and [Track Conversions](../../c-implementing-target/c-implementing-target-for-client-side-web/how-to-deployatjs/implementing-target-without-a-tag-manager.md#task_E85D2F64FEB84201A594F2288FABF053).

Here are the API details:

| Key | Type | Required | Description |
|--- |--- |--- |--- |
|mbox|String|Yes|Mbox name|
|selector|String|No|CSS selectors used to find the HTML elements. The event listeners will be attached to found elements.|
|type|String|No|Represents a registered event type. It can be both HTML known events like: click, mousedown ,etc., as well as custom HTML events.|
|preventDefault|Boolean|No|Indicates whether to use `event.preventDefault()` in the event listener callback. Defaults to false.<br>**Note**: Only `form[submit] and `a[click]` are supported. Other scenarios are not supported due to complexity and huge amount of scenarios to support.|
|params|Object|No|Mbox parameters. An object of key-value pairs that has the following structure:<br>`{ "param1": "value1", "param2": "value2"}`|
|timeout|Number|No|Timeout in milliseconds.<br>If not specified, default value is used:<br>`...timeoutInSeconds: 0.15...}`|
|success|Function|No|A callback function used to signal that event has been reported.|
|error|Function|No|A callback function used to signal that event could not be reported.|

### Example

```
<a href="https://asite.com">click me!</a> 
```

plus javaScript code to assign `trackEvent`:

```
<script> 
$('a').click(function(event){ 
  adobe.target.trackEvent({'mbox':'homePageHero'}) 
}); 
</script> 
```

Or:

```
adobe.target.trackEvent({ 
    "mbox": "clicked-cta", 
    "params": { 
        "param1": "value1" 
    } 
});
```

>[!NOTE]
>
>In case the mandatory fields are not set, no request is executed, and an error is thrown.

## mboxCreate(mbox,params) - at.js 1.x {#reference_E68805FE86C64792B2066DB17B253D74}

Executes a request and applies the offer to the closest DIV with mboxDefault class name.

>[!NOTE]
>
>This function is available for at.js versions 1.*x* only. This function was deprecated with the release of at.js 2.0. This function returns default content if used with at.js 2.0.

This function is built into [!DNL at.js] mostly to ease the transition from [!DNL mbox.js] to [!DNL at.js]. A newer alternative to `mboxCreate()` is `adobe.target.getOffer()`/ `adobe.target.applyOffer()` or the Angular directive.

**Example**

```
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  mboxCreate('mboxName','param1=value1','param2=value2'); 
</script>
```

**Notes**

`mboxCreate()` now uses the "json" endpoint instead of the "standard" endpoint and fires asynchronously. Because of this:

* [Debugging](../../c-implementing-target/c-implementing-target-for-client-side-web/c-target-debugging-atjs/target-debugging-atjs.md#concept_CAE591DA8C404C22917584ECD4F7494F) is a little different. 
* Avoid offer code requiring synchronous, blocking calls.

  For example, offers that set JavaScript variables that are used by site code or other mboxes that come later on the page. 
* Be sure to have a `<div class="mboxDefault"></div>`before invoking `mboxCreate()`, because [!DNL at.js] will not add one for you. 

* Empty, top-of-page `mboxCreate()` functions are not recommended as a global mbox.

  The auto-created global mbox in [!DNL at.js] is a better option because it fires from the `<head>` and can return content earlier.

## mboxDefine() and mboxUpdate() - at.js 1.x {#reference_61B2B9F351344CF5B0915D5AFD21C5FE}

Define and update an mbox.

>[!NOTE]
>
>These functions are available for at.js versions 1.*x* only. These functions were deprecated with the release of at.js 2.0. These functions return default content if used with at.js 2.0.

`mboxDefine()` and `mboxCreate()` are tied to HTML DIV elements where the offer should be displayed. These HTML DIV elements should have the `mboxDefault` class. If the HTML elements won't have this class attached, you could see some noticeable flicker.

### mboxDefine {#section_134BAAE8EE9D49D8BAFEA5E7EAB93BA7}

Creates an internal mapping between a nodeId and an mbox name, but does not execute the request. Used in conjunction with `mboxUpdate()`. Built into [!DNL at.js]mostly to ease the transition from [!DNL mbox.js]to [!DNL at.js].

### mboxUpdate {#section_D20B3E551884452A996305C12D5959D5}

Executes the request and applies the offer to the element identified by the `nodeId` in the `mboxDefine()`. Can also be used to update an mbox initiated by `mboxCreate`. Built into [!DNL at.js] mostly to ease the transition from [!DNL mbox.js] to [!DNL at.js]. `mboxDefine()`/ `mboxUpdate()` could be replaced by [ `adobe.target.getOffer()` ](../../c-implementing-target/c-implementing-target-for-client-side-web/cmp-at.js-functions.md#reference_C81525D1598A4A1199740DCAB81A7FDF)and [ `adobe.target.applyOffer()` ](../../c-implementing-target/c-implementing-target-for-client-side-web/cmp-at.js-functions.md#reference_BBE83F513B5B4E03BBC3F50D90864245) using the selector option.

### Example {#section_9C1E75D9E4BA4DC7879D2B69877EB01A}

```
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```

## targetGlobalSettings() {#concept_8DACBC47ABDE4279BB102B42609FE506}

You can override settings in the at.js library using `targetGlobalSettings()`, rather than configuring the settings in the [!DNL Target] Standard/Premium UI or by using REST APIs.

There are use cases, especially when at.js is delivered via [!DNL Dynamic Tag Management] (DTM) when you would like to override some of the settings.

### Settings {#section_42C759AE9B524A43B8659018677224B8}

You can override the following settings:

| Settings | Type | Default Value | Description |
|--- |--- |--- |--- |
|clientCode|String|Value set via UI|Represents client code|
|serverDomain|String|Value set via UI|Represents Target edge server|
|cookieDomain|String|If possible set to top level domain|Represents the domain used when saving cookies|
|crossDomain|String|Value set via UI|Indicates whether cross-domain tracking is enabled or not.<br>The allowed values are:<ul><li>disabled</li><li>enabled</li><li>x-only</li></ul>|
|timeout|Number|Value set via UI|Represents Target edge request timeout|
|globalMboxAutoCreate|Boolean|Value set via UI|Indicates whether the global mbox request should be fired or not|
|visitorApiTimeout|Number|2000 ms = 2 s|Represents the Visitor API request timeout|
|enabled|Boolean|true|Indicates whether at.js as library is enabled, meaning if it should execute anything or not. The main use case for this setting being opt-out cookies or other custom decisions that would disable  at.js  functionality|
|defaultContentHiddenStyle|String|visibility: hidden|Used only for wrapping mboxes that use DIV with class name "mboxDefault" and are executed via `mboxCreate()`, `mboxUpdate()`, or `mboxDefine()` to hide default content|
|defaultContentVisibleStyle|String|visibility: visible|Used only for wrapping mboxes that use DIV with class name "mboxDefault" and are executed via `mboxCreate()`, `mboxUpdate()`, or `mboxDefine()` to reveal applied offer if any or default content|
|bodyHiddenStyle|String|body { opacity: 0 }|Used only when `globalMboxAutocreate === true` to minimize the chance of flicker.<br>For more information, see [How at.js Manages Flicker](/help/c-implementing-target/c-implementing-target-for-client-side-web/c-how-atjs-works/manage-flicker-with-atjs.md).|
|bodyHidingEnabled|Boolean|true|Used to control flicker when `target-global-mbox` is used to deliver offers created in the  Visual Experience Composer , also known as visual offers|
|imsOrgId|String|IMS ORG ID|Represents the IMS ORG ID|
|secureOnly|Boolean|false|Indicates whether  at.js  should use HTTPS only or be allowed to switch between HTTP and HTTPS based on the page protocol.|
|overrideMboxEdgeServer|Boolean|true (true beginning with at.js version 1.6.2)|Indicates if we should use `<clientCode>.tt.omtrdc.net` domain or `mboxedge<clusterNumber>.tt.omtrdc.net` domain.<br>If this value is true, `mboxedge<clusterNumber>.tt.omtrdc.net` domain will be saved to a cookie|
|overrideMboxEdgeServerTimeout|Number|1860000 => 31 minutes|Indicates the cookie lifetime that contains the `mboxedge<clusterNumber>.tt.omtrdc.net` value.|
|optoutEnabled|Boolean|false|Indicates whether Target should call the Visitor API `isOptedOut()` function. This is part of Device Graph enablement.|
|selectorsPollingTimeout|Number|5000 ms = 5 s|In at.js 0.9.6, Target introduced this new setting that can be overridden via `targetGlobalSettings`.<br>`selectorsPollingTimeout` represents how long the client is willing to wait for all the elements identified by selectors to appear on the page.<br>Activities created via the Visual Experience Composer (VEC) have offers that contain selectors.|
|dataProviders|See "Data Providers" below.|See "Data Providers" below.|See "Data Providers" below.|

### Usage {#section_9AD6FA3690364F7480C872CB55567FB0}

This function can be defined before at.js is loaded or in **[!UICONTROL Setup]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit at.js Settings]** > **[!UICONTROL Code Settings]** > **[!UICONTROL Library Header]**.

The Library Header field allows you to enter free-form JavaScript. The customization code should look something similar to the following example:

```
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

### Data Providers {#section_42725F3C837247D58AE1831EA330E44D}

This setting lets customers gather data from third-party data providers, such as Demandbase, BlueKai, and custom services, and pass the data to Target as mbox parameters in the global mbox request. It supports the collection of data from multiple providers via async and sync requests. Using this approach makes it easy to manage flicker of default page content, while including independent timeouts for each provider to limit the impact on page performance

>[!NOTE]
>
>Data Providers requires [!DNL at.js] 1.3 or later.

The following videos contain more information:

| Video | Description |
|--- |--- |
|[Using Data Providers in Adobe Target](https://helpx.adobe.com/target/kt/using/dataProviders-atjs-feature-video-use.html)|Data Providers is a capability that allows you to easily pass data from third parties to Target. A third party could be a weather service, a DMP, or even your own web service. You can then use this data to build audiences, target content, and enrich the visitor profile.|
|[Implement Data Providers in Adobe Target](https://helpx.adobe.com/target/kt/using/dataProviders-atjs-technical-video-implement.html)|Implementation details and examples of how to use Adobe Target's dataProviders feature to retrieve data from third-party data providers and pass it in the Target request.|

The `window.targetGlobalSettings.dataProviders` setting is an array of data providers.

Each data provider has the following structure:

| Key | Type | Description |
|--- |--- |--- |
|name|String|Name of provider.|
|version|String|Provider version. This key will be used for provider evolution.|
|timeout|Number|Represents the provider timeout if this is a network request.  This key is optional.|
|provider|Function|The function that contains the provider data fetching logic.<br>The function has a single required parameter: `callback`. The callback parameter is a function that should be invoked only when the data has been successfully fetched or there is an error.<br>The callback expects two parameters:<ul><li>error: Indicates if an error occurred. If everything is OK, then this parameter should be set to null.</li><li>params: A JSON object, representing the parameters that will be sent in a Target request.</li></ul>|

The following example shows where the data provider is using sync execution:

```
var syncDataProvider = { 
  name: "simpleDataProvider", 
  version: "1.0.0", 
  provider: function(callback) { 
    callback(null, {t1: 1}); 
  } 
}; 
  
window.targetGlobalSettings = { 
  dataProviders: [ 
    syncDataProvider 
  ] 
};
```

After at.js processes `window.targetGlobalSettings.dataProviders`, the Target request will contain a new parameter: `t1=1`.

The following is an example if the parameters that you want to add to the Target request are fetched from a third-party service, such as Bluekai, Demandbase, and so forth:

```
var blueKaiDataProvider = { 
   name: "blueKai", 
   version: "1.0.0", 
   provider: function(callback) { 
      // simulating network request 
     setTimeout(function() { 
       callback(null, {t1: 1, t2: 2, t3: 3}); 
     }, 1000); 
   } 
} 
  
window.targetGlobalSettings = { 
   dataProviders: [ 
      blueKaiDataProvider 
   ] 
};
```

After at.js processes `window.targetGlobalSettings.dataProviders`, the Target request will contain additional parameters: `t1=1`, `t2=2` and `t3=3`.

The following example uses data providers to collect weather API data and send it as parameters in a Target request. The Target request will have additional params, such as `country` and `weatherCondition`.

```
var weatherProvider = { 
      name: "weather-api", 
      version: "1.0.0", 
      timeout: 2000, 
      provider: function(callback) { 
        var API_KEY = "caa84fc6f5dc77b6372d2570458b8699"; 
        var lat = 44.426767399999996; 
        var lon = 26.1025384; 
        var url = "//api.openweathermap.org/data/2.5/weather?lang=en"; 
        var data = { 
          lat: lat, 
          lon: lon, 
          appId: API_KEY 
        } 
 
        $.ajax({ 
          type: "GET", 
                url: url, 
          dataType: "json", 
          data: data, 
          success: function(data) { 
            console.log("Weather data", data); 
            callback(null, { 
              country: data.sys.country, 
              weatherCondition: data.weather[0].main 
            }); 
          }, 
          error: function(err) { 
            console.log("Error", err); 
            callback(err); 
          } 
        });         
      } 
    }; 
 
    window.targetGlobalSettings = { 
      dataProviders: [weatherProvider] 
    };
```

Consider the following when working with the `dataProviders` setting:

* If the data providers added to `window.targetGlobalSettings.dataProviders` are async, they will be executed in parallel. The Visitor API request will be executed in parallel with functions added to `window.targetGlobalSettings.dataProviders` to allow a minimum wait time. 
* at.js won't try to cache the data. If the data provider fetches data only once, the data provider should make sure that data is cached and, when the provider function is invoked, serve the cache data for the second invocation.

## targetPageParams() {#reference_B235C9F6DA79449ABE3E23F914FEABAE}

This method allows you to attach parameters to the global mbox from outside of the request code.

This function is very useful for including the same set of parameters on multiple mbox calls. The function needs to be defined by the customer. It should return an array of parameters that will be passed only to the global mbox request. This function can be defined before at.js is loaded or in **[!UICONTROL Setup]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit at.js Settings]** > **[!UICONTROL Code Settings]** > **[!UICONTROL Library Header]**.

You can pass in parameters to target-global-mbox using the `targetPageParams()` function in any of the following ways:

* An ampersand-delimited list 
* An array 
* A JSON object

### Examples

Ampersand-delimited list (values must be URL encoded):

```
function targetPageParams() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (values do not need to be URL encoded):

```
targetPageParams = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (values do not need to be URL encoded):

```
targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
``` 

## targetPageParamsAll() {#reference_97E77FCDD793403685ECCA5A44305F93}

This method allows you to attach parameters to all mboxes from outside of the request code.

This is very useful for including the same set of parameters on multiple mbox calls. The function needs to be defined by the customer. It should return an array of parameters that will be passed to all mbox requests on the page. This function can be defined before at.js is loaded or in **[!UICONTROL Setup]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit at.js Settings]** > **[!UICONTROL Code Settings]** > **[!UICONTROL Library Header]**.

You can pass in parameters to target-global-mbox using the targetPageParamsAll() function in any of the following ways:

* An ampersand-delimited list 
* An array 
* A JSON object

### Examples

Ampersand-delimited list (values must be URL encoded):

```
function targetPageParamsAll() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (values do not need to be URL encoded):

```
targetPageParamsAll = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (values do not need to be URL encoded):

```
targetPageParamsAll = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
``` 

## registerExtension() - at.js 1.x {#reference_B3ACC004D45E460C8DD94C1476D2625C}

Provides a standard way to register a specific extension.

>[!NOTE]
>
>This function is available for at.js versions 1.*x* only. This function was deprecated with the release of at.js 2.0. This function returns default content if used with at.js 2.0.

The options parameter is mandatory and has the following structure:

| Key | Type | Required | Description |
|--- |--- |--- |--- |
|name|String|Yes|Extension name.|
|modules|Array[String]|Yes|An array of strings representing requested module names.|
|register|Function|Yes|A function used to initialize and build the extension. This function receives arguments based on modules array.|

Notes:

* If one of the parameters is not provided, an exception is thrown. 
* If the modules array is empty, an exception is thrown.

For more information and examples of how to use `registerExtension`, see the [Adobe Experience Cloud Target atjs Extensions](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions) page on GitHub.

### Settings Module Methods {#section_8501CDD4B0624FA2B10532C98C5F4328}

| Key | Type | Description |
|--- |--- |--- |
|clientCode|String|Client code|
|serverDomain|String|Edge server domain|
|globalMboxName|String|Target global mbox name|
|globalMboxAutoCreate|Boolean|Indicates if auto-create is enabled or not|
|timeout|Number|Request timeout|

### Logger Module Methods {#section_10AF62B49AEF48F981E950D26E176138}

| Key | Type | Description |
|--- |--- |--- |
|log|Function|Logs the variable list of arguments to the browser console, if it exists. It is activated only when `mboxDebug=true` is passed to the URL.|
|error|Function|Logs the variable list of arguments to the browser console. It is activated only when there are serious errors, such as network timeout, HTML node not found, etc.|

## at.js custom events {#reference_A828E4BA535F4E7692A075F3D70CF6CD}

Information about `at.js custom events`, which lets you know when an mbox request or offer fails or succeeds.

Historically, mbox.js didn't let other JavaScript code that runs on the page know what happens behind the scenes. With the advancement of at.js, we had a unique opportunity to fix this issue.

According to our customers there are several scenarios that they would like to be notified of, including:

* An mbox request failed due to timeout, wrong status code, JSON parse error, etc. 
* An mbox request succeeded. 
* Offer rendering failed due to wrapping mbox element missing, selector can not be found, etc. 
* Offer rendering succeeded. DOM changes have been applied.

Pre-defined events have a structure that allows you to extract the required data, based on event type.

To make sure that events can be used in different scenarios, the custom events have a payload object that is assigned to the detail property of the event object (that is passed to the handler). Also to avoid passing strings as event names, the events are exposed as constants using `adobe.target.event` namespace.

### Structure {#section_0E5C9A13DE234A5DAECBCBF9F23F94FE}

| Key | Type | Description |
|--- |--- |--- |
|type|String|There are several scenarios in which you would like to be notified to help in tracing, debugging, and customizing interaction with at.js.<br>Each custom event listed below has two formats: a "constant" and a "string value."<ul><li>**Constants**: Prepended with `adobe.target.event.`, presented in all caps, and contain underscore characters. To subscribe to custom events *after* at.js loads but *before* the mbox response has been received, use the constant.</li><li>**String Values**: Lowercase and contain dashes. To subscribe to custom events *before* at.js loads, use the string value.</li></ul>**Request Failed**<br>Constant: `adobe.target.event.REQUEST_FAILED`<br>Sting value: `at-request-failed`<br>Description: An mbox request failed due to timeout, wrong status code, JSON parse error, etc.<br>**Request Succeeded**<br>Constant: `adobe.target.event.REQUEST_SUCCEEDED`<br>String Value: `at-request-succeeded`<br>Description: An mbox request was successful.<br>**Content Rendering Failed**<br>Constant: `adobe.target.event.CONTENT_RENDERING_FAILED`<br>String Value: `at-content-rendering-failed`<br>Description: Offer rendering failed due to wrapping mbox element missing, selector can not be found, etc.<br>**Content Rendering Succeeded**<br>Constant: `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<br>String Value: `at-content-rendering-succeeded`<br>Description: Offer rendering was successful. DOM changes have been applied.<br>**Library Loaded**<br>Constant: `adobe.target.event.LIBRARY_LOADED`<br>String Value: `at-library-loaded`<br>Description: This event is ideal to track when at.js has been fully loaded. You can use this event to customize global mbox execution. You can also use this event to disable the global mbox and then listen for this event to fire the global mbox later.<br>**Request Start**<br>Constant: `adobe.target.event.REQUEST_START`<br>String Value: `at-request-start`<br>Description: This event is fired before an HTTP request is executed. You can use this event for performance measurements using the Resource Timing API.<br>**Content Rendering Start**<br>Constant: `adobe.target.event.CONTENT_RENDERING_START`<br>String Value: `at-content-rendering-start`<br>Description: This event is fired before selector polling is started and content is rendered to the page. You can use this event to track the content rendering progress.<br>**Content Rendering No Offers**<br>Constant: `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<br>String Value: `at-content-rendering-no-offers`<br>Description: This event is fired when there are no offers returned.<br>**Content Rendering Redirect**<br>Constant: `adobe.target.event.CONTENT_RENDERING_REDIRECT`<br>String Value: `at-content-rendering-redirect`<br>Description: This event is fired when an offer is a redirect and Target will redirect to a different URL.|
|mbox|String|mbox name|
|message|String|Contains human-readable description, such as what happened, the error message, etc.|
|tracking|Object|Contains the `sessionId` and `deviceId`. In some cases, `deviceId` could be missing because [!DNL Target] couldn't retrieve it from edge server.|

### Usage {#section_0500FF09D3A04450B5DC8F85C6F793E0}

```
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## Training Video: Response Tokens and the at.js Custom Events {#section_ED304A7137DC42A4BDCD6D57C989F1FA}

Watch the following video to learn how to use Response Tokens and at.js Custom Events to share profile information from Target to third-party systems.

>[!VIDEO](https://video.tv.adobe.com/v/23253/)