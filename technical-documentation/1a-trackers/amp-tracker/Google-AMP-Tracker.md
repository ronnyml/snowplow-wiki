[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers)

Snowplow is collaborating with Google on their [Accelerated Mobile Pages Project] [amp] (AMPP or AMP for short). AMP is an open source (Apache License 2.0) initiative to improve the mobile web experience by optimizing web pages for mobile devices.

Snowplow is natively integrated in the project, so pages optimized with AMP HTML can be tracked in Snowplow by adding the appropriate `amp-analytics` tag to your pages:

```html
<body>
  ...
  <amp-analytics type="snowplow">
  <script type="application/json">
  {
    "vars": {
      "collectorUrl": "COLLECTOR URL"
    },
   ...
  }
  </script>
  </amp-analytics>
</body>
```

Please make sure to include the URI to your collector 
[amp]: https://www.ampproject.org/