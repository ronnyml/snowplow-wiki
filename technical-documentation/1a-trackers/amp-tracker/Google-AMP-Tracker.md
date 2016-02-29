[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers)

## 1. Overview

Snowplow is collaborating with Google on their [Accelerated Mobile Pages Project] [amp] (AMPP or AMP for short). AMP is an open source (Apache License 2.0) initiative to improve the mobile web experience by optimizing web pages for mobile devices.

To learn more about analytics for AMP pages see the [amp-analytics] [amp-analytics] reference. For general information about AMP see [What Is AMP?] [amp-what] on the [Accelerated Mobile Pages (AMP) Project] [amp] site.

Snowplow is natively integrated in the project, so pages optimized with AMP HTML can be tracked in Snowplow by adding the appropriate `amp-analytics` tag to your pages:

```html
<body>
  ...
  <amp-analytics type="snowplow" id="snowplow1">
  <script type="application/json">
  {
    ...
  }
  </script>
  </amp-analytics>
</body>
```

## 2. Standard variables

Certain parameters must be provided in the `"vars"` section of the tag:

```javascript
{
  "vars": {
    ...
  },
 ...
}
```

### 2.1 `collectorHost`

Specify the host to your collector like so:

```javascript
  "vars": {
    "collectorHost": "snowplow-collector.acme.com"
    ...
```

Notes:

* Do *not* include a trailing slash
* Use of HTTPS is mandatory in AMP, so your Snowplow collector **must** support HTTPS

### 2.2 `appId`

TO WRITE

## 3. Specific event support

The following trigger request values are supported for the Google Analytics configuration:

 * `pageView` for page view tracking
 * `unstructEvent` for structured event tracking

All event tracking is disabled by support; you can enable it on an event-by-event basis as follows:

### 3.1 Page view

Enable the page view tracking like so:

```html
<amp-analytics type="snowplow" id="snowplow1">
<script type="application/json">
{
  "vars": {
    "collectorHost": "snowplow-collector.acme.com"  // Replace with your collector host
  },
  "triggers": {
    "trackPageview": {  // Trigger names can be any string. trackPageview is not a required name
      "on": "visible",
      "request": "pageview"
    }
  }
}
</script>
</amp-analytics>
```

### 3.2 Structured events

[amp]: https://www.ampproject.org/
[amp-what]: https://www.ampproject.org/docs/get_started/about-amp.html
[amp-analytics]: https://www.ampproject.org/docs/reference/extended/amp-analytics.html
