<a name="top" />

[**HOME**](Home) > [**SNOWPLOW TECHNICAL DOCUMENTATION**](Snowplow technical documentation) > [**Trackers**](trackers) > [**JavaScript Tracker**](Javascript-Tracker) > Specific event tracking

<a name="tracking-specific-events" />
## 4. Advanced usage

This page covers the more advanced elements of the Snowplow JavaScript Tracker API.

  - 4.1 [Callbacks](#callbacks)

<a name="callbacks" />
### 4.1 Callbacks

If you call `snowplow_name_here` with a function as the argument, the function will be executed when sp.js loads:

```javascript
snowplow_name_here(function () {
  console.log("sp.js has loaded");
});
```

Or equivalently:

```javascript
snowplow_name_here(function (x) {
  console.log(x);
}, "sp.js has loaded");
```

The callback you provide is executed as a method on the internal `trackerDictionary` object. This means that you can access the `trackerDictionary` using `this`:

```javascript
// Configure a tracker instance named "cf"
snowplow('newTracker', 'cf', 'd3rkrsqld9gmqf.cloudfront.net', {
	appId: 'snowplowExampleApp',
	platform: 'web'
});

// Access the tracker instance inside a callback
snowplow(function () {
	var cf = this.cf;
	var userFingerprint = cf.getUserFingerprint();
	doSomethingWith(userFingerprint);
})
```

The callback function should not be a method:

```javascript
// TypeError: Illegal invocation
snowplow_name_here(console.log, "sp.js has loaded");
```

will not work, because the value of `this` in the `console.log` function will be `trackerDictionary` rather than `console`.

You can get around this problem using `Function.prototoype.bind` as follows:

```javascript
snowplow_name_here(console.log.bind(console), "sp.js has loaded");
```

For more on execution context in JavaScript, see the [MDN page][execution-context].

<a name="return-methods" />
### 4.2 Methods which can be used from inside a callback

<a name="get-user-fingerprint" />
#### 4.2.1 `getUserFingerprint`

The `getUserFingerprint` method returns the tracker-generated user fingerprint:

```javascript
// Configure a tracker instance named "cf"
snowplow('newTracker', 'cf', 'd3rkrsqld9gmqf.cloudfront.net', {
	appId: 'snowplowExampleApp',
	platform: 'web'
});

// Access the tracker instance inside a callback
snowplow(function () {
	var cf = this.cf;
	var userFingerprint = cf.getUserFingerprint();
	doSomethingWith(userFingerprint);
})
```

<a name="get-domain-user-id" />
#### 4.2.1 `getDomainUserId`

The `getDomainUserId` method returns the user ID stored in the first-party cookie:

```javascript
// Access the tracker instance inside a callback
snowplow(function () {
	var cf = this.cf;
	var domainUserId = cf.getDomainUserId();
	doSomethingWith(domainUserId);
})
```

<a name="get-domain-user-info" />
#### 4.2.1 `getDomainUserInfo`

The `getDomainUserInfo` method returns all the information stored in first-party cookie in an array:

```javascript
// Access the tracker instance inside a callback
snowplow(function () {
	var cf = this.cf;
	var domainUserInfo = cf.getDomainUserInfo();
	doSomethingWith(domainUserInfo);
})
```

The `domainUserInfo` variable will contain an array with 6 elements:

0. A string set to `'1'` if this is the user's first session and `'0'` otherwise
1. The domain user ID
2. The timestamp at which the cookie was created
3. The number of times the user has visited the site
4. The timestamp for the current visit
5. The timestamp of the last visit

<a name="get-user-id" />
#### 4.2.1 `getUserId`

The `getUserId` method returns the user ID which you configured using `setUserId()`:

```javascript
// Access the tracker instance inside a callback
snowplow(function () {
	var cf = this.cf;
	var userId = cf.getUserId();
	doSomethingWith(userId);
})
```

[execution-context]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this
