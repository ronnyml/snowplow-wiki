[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers)

## 1. Overview

Snowplow is collaborating with Google on their [Accelerated Mobile Pages Project] [amp] (AMPP or AMP for short). AMP is an open source (Apache License 2.0) initiative to improve the mobile web experience by optimizing web pages for mobile devices.

Snowplow is natively integrated in the project, so pages optimized with AMP HTML can be tracked in Snowplow by adding the appropriate `amp-analytics` tag to your pages:

```html
<body>
  ...
  <amp-analytics type="snowplow">
  <script type="application/json">
  {
    ...
  }
  </script>
  </amp-analytics>
</body>
```

## 2. Standard tagging

Certain parameters must be provided for AMP to correctly send events to Snowplow.

In the `"vars"` section you must specify your collector host:

```javascript
{
  "vars": {
    "collectorHost": "snowplow-collector.acme.com"
  },
 ...
}
```

Use of HTTPS is mandatory in AMP, therefore your Snowplow collector **must** support HTTPS. 

## 3. Specific event support

All event tracking is disabled by support; you can enable it on an event-by-event basis as follows:

### 3.1 Page view

### 3.2 Structured events

[amp]: https://www.ampproject.org/