<a name="top" />

[**HOME**](Home) > [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) > [**Step 2: setup a Tracker**](Setting-up-a-Tracker) > [**Node.js tracker**](Nodejs-tracker-setup)

## Contents

- 1. [Installation](#installation)  
- 2. [Next steps](#next-steps)

<a name="installation" />
## 1. Installation

The [Snowplow Node.js Tracker](https://github.com/snowplow/snowplow-nodejs-tracker) lets you add analytics to your [Node.js][node-js]-based applications.

Setting up the tracker should be straightforward if you are familiar with [npm][npm]:

```bash
npm install snowplow-tracker
```

You can automatically save the Tracker as a dependency for your npm module like this:

```bash
npm install snowplow-tracker --save
```

<a name="next-steps" />
## 2. Next steps

Once you've installed the module, check out the [technical documentation page](Node.js-Tracker) for information on the Node.js Tracker API. Then you should be ready to start tracking your own events.

[Back to top](#top)

[node-js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
