<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2: setup a Tracker**](Setting-up-a-Tracker) > [**JavaScript Tracker**](Javascript-tracker-setup) > [Integrating JavaScript tags with enhanced ecommerce](Integrating-Javascript-tags-with-enhanced-ecommerce)

**Warning: this feature is in test and should not yet be used.**

1. [Overview](#overview)
2. [Creating the Data Layer Variable](#variable)
3. [Creating the trigger](#overview)
4. [Writing the JavaScript](#script)
5. [Creating the tag](#tag)

<a name="overview" />
### 1. Overview

This guide will show you how to configure Google Tag Manager to load the Snowplow JavaScript Tracker and send enhanced ecommerce data to Snowplow as well as Google without changing any of your calls to `dataLayer.push`. We will assume that you have already implemented enhanced ecommerce in Universal Analytics as described in the [Enhanced Ecommerce (UA) Developer Guide][enhancedEcommerceDeveloperGuide].

We also assume that any ecommerce-related call to `dataLayer.push` which does not contain an "event" field is made before Google Tag Manager loads, as described [here](http://www.simoahava.com/analytics/ecommerce-tips-google-tag-manager/#tip1).

<a name="variable" />
### 2. Creating the Data Layer Variable

In the Variables tab, create a Data Layer Variable. Set the name of this variable to "ecommerce". This variable will hold all ecommerce-related data and will be updated when you call `dataLayer.push` with a JSON containing the key "ecommerce".

<a name="trigger" />
### 3. Creating the trigger

The trigger will detect ecommerce data pushed into the data layer and cause the main tag to fire.

In the Triggers tab, Create a new trigger named "Enhanced Ecommerce". In the "Choose Event" section, choose "Custom Event". Set "Fire On" to the string `gtm.dom|checkout|checkoutOption|productClick|addToCart|removeFromCart|promotionClick` and check the "Use regex matching" box.

<a name="script" />
### 4. Writing the JavaScript

Your tag will fire both when the page loads and also every time an ecommerce event is pushed to the data layer.

When the page loads, the tag will load the Snowplow JavaScript Tracker, make the API calls necessary to set up tracking. If the data layer contains ecommerce data, like product impressions, the tag will also send that data to Snowplow.

Whenever ecommerce data is pushed to the data layer, the tag will fire again. It will not attempt to set up tracking again; instead it will send the ecommerce event to Snowplow.

The example script below will be used as the basis for your tag. There are some changes you should make to this script. In the example "SNOWPLOW_NAME_HERE" is used as the name of the Snowplow function. This is the global function used to make API calls to the Snowplow JavaScript Tracker. You should change this string to something unique so that if there is another Snowplow user on the page the namespaces will not collide. You should change "MY_COLLECTOR" to the URL of your Snowplow collector (minus the http/https scheme), for example "mycloudfrontsubdomain.cloudfront.net".

You can also customize the part of the tag between the comments containing "!!!". The example below creates a tracker instance, sets page pings to fire every 10 seconds, and sends a page view event. See the rest of the JavaScript Tracker documentation for more information on other tracking methods.

```html
<script>
  // If this tag fires more than once (e.g. page view followed by ecommerce action),
  // we don't want to repeat the trackPageView here
  if (!window.SNOWPLOW_NAME_HERE) {
    ;(function(p,l,o,w,i,n,g){if(!p[i]){p.GlobalSnowplowNamespace=p.GlobalSnowplowNamespace||[];
    p.GlobalSnowplowNamespace.push(i);p[i]=function(){(p[i].q=p[i].q||[]).push(arguments)
    };p[i].q=p[i].q||[];n=l.createElement(o);g=l.getElementsByTagName(o)[0];n.async=1;
    n.src=w;g.parentNode.insertBefore(n,g)}}(window,document,"script","//d1fc8wv8zag5ca.cloudfront.net/2.5.2/sp.js","SNOWPLOW_NAME_HERE"));


    // !!! Customizable section starts
    // Track page views, enable link clicks, and so on here
      
    SNOWPLOW_NAME_HERE('newTracker', 'snplow1', 'MY_COLLECTOR', {
      'appId': 'snowplowweb',
      'contexts': {
        'webPage': true,
        'performanceTiming': true
      }
    });

	SNOWPLOW_NAME_HERE('enableActivityTracking', 10, 10);
    SNOWPLOW_NAME_HERE('trackPageView');

    // !!! Customizable section ends
    
  }
  
  var ecommerce = {{ecommerce}};

  var actions = [
    "click",
    "detail",
    "add",
    "remove",
    "checkout",
    "checkout_option",
    "purchase",
    "refund",
    "promo_click",
    "view"
  ];

  if (ecommerce) {
    sendEnhancedEcommerceEvent(ecommerce);
  }
  
  function sendEnhancedEcommerceEvent(ecommerce) {
    var currencyCode = ecommerce.currencyCode;
    var action;
    var contexts = [];
    for (var i=0;i<actions.length;i++) {
      if (ecommerce[actions[i]]) {
        action = actions[i];
      }
    }
    if (ecommerce.impressions) {
      for (var j=0;j<ecommerce.impressions.length;j++) {
        contexts.push(getImpressionContext(ecommerce.impressions[j]));
      }
    }

    if (ecommerce.promoView) {
      for (var l=0;l<ecommerce.promoView.promotions.length;l++) {
        contexts.push(getPromoContext(ecommerce.promoView.promotions[l]));
      }
    }
    if (! action) {
      action = 'view';
    } else {
      if (ecommerce[action].products) {
        for (var k=0;k<ecommerce[action].products.length;k++) {
          contexts.push(getProductContext(ecommerce[action].products[k]));
        }
      }
      if (ecommerce[action].actionField) {
        contexts.push(getActionContext(ecommerce[action].actionField));
      }
    }

    var unstructEvent = getActionJson(action);
    finalizeJsons([unstructEvent]);
    finalizeJsons(contexts, currencyCode);

    SNOWPLOW_NAME_HERE('trackUnstructEvent', unstructEvent, contexts);
  }

  function finalizeJsons(jsons, currencyCode) {
    for (var i=0; i<jsons.length; i++) {
      var data = jsons[i].data;
      if (currencyCode) {
        data.currency = currencyCode;
      }
      for (var j in data) {
        if (data[j] === null || data[j] === undefined || data[j] === NaN) {
          delete data[j];
        }
      }
    }
  }

  function getActionJson(action) {
    return {
      schema: 'iglu:com.google.analytics.enhanced-ecommerce/action/jsonschema/1-0-0',
      data: {
        action: action
      }
    };
  }
  
  function getImpressionContext(impression) {
    var data = {
      name: impression.name,
      id: impression.id,
      price: parseFloat(impression.price),
      brand: impression.brand,
      category: impression.category,
      variant: impression.variant,
      list: impression.list,
      position: parseInt(impression.position) 
    };
    return {
      schema: 'iglu:com.google.analytics.enhanced-ecommerce/impressionFieldObject/jsonschema/1-0-0',
      data: data
    };
  }
  
  function getProductContext(product) {
    var data = {
      name: product.name,
      id: product.id,
      price: parseFloat(product.price),
      quantity: parseInt(product.quantity),
      coupon: product.coupon,
      position: parseInt(product.position),
      brand: product.brand,
      category: product.category,
      variant: product.variant
    };
    return {
      schema: 'iglu:com.google.analytics.enhanced-ecommerce/productFieldObject/jsonschema/1-0-0',
      data: data
    };
  }

  function getPromoContext(promo) {
    var data = {
      name: promo.name,
      id: promo.id,
      creative: promo.creative,
      position: promo.position
    }
    return {
      schema: 'iglu:com.google.analytics.enhanced-ecommerce/promoFieldObject/jsonschema/1-0-0',
      data: data
    };
  }

  function getActionContext(action) {
    var data = {
      id: action.id,
      affiliation: action.affiliation,
      revenue: parseFloat(action.revenue),
      tax: parseFloat(action.tax),
      shipping: parseFloat(action.shipping),
      coupon: action.coupon,
      list: action.list,
      step: parseInt(action.step),
      option: action.option
    }
    return {
      schema: 'iglu:com.google.analytics.enhanced-ecommerce/actionFieldObject/jsonschema/1-0-0',
      data: data
    };
  }

</script>
```

<a name="tag" />
### 5. Creating the tag

In the Tags tab, create a new tag. Name it something like "Enhanced Ecommerce Pageview". In the "Choose Product" section, select "Custom HTML Tag". Copy and paste the JavaScript you wrote in the previous section into the textbox.

From the Fire On section, choose "More", then select the "Enhanced Ecommerce" trigger you created earlier and save.

[enhancedEcommerceDeveloperGuide]: https://developers.google.com/tag-manager/enhanced-ecommerce