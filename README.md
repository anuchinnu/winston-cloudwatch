# winston-cloudwatch [v2.3.0](https://github.com/lazywithclass/winston-cloudwatch/blob/master/CHANGELOG.md#230)

[![Build Status](https://travis-ci.org/lazywithclass/winston-cloudwatch.svg?branch=master)](https://travis-ci.org/lazywithclass/winston-cloudwatch) [![Coverage Status](https://coveralls.io/repos/github/lazywithclass/winston-cloudwatch/badge.svg?branch=master)](https://coveralls.io/github/lazywithclass/winston-cloudwatch?branch=master) [![Dependency Status](https://david-dm.org/lazywithclass/winston-cloudwatch.svg)](https://david-dm.org/lazywithclass/winston-cloudwatch) [![dev dependencies](https://david-dm.org/lazywithclass/winston-cloudwatch/dev-status.svg)](https://david-dm.org/lazywithclass/winston-cloudwatch#info=devDependencies) [![peer dependencies](https://david-dm.org/lazywithclass/winston-cloudwatch/peer-status.svg)](https://david-dm.org/lazywithclass/winston-cloudwatch#info=peerDependencies)
==================

Send logs to Amazon Cloudwatch using [Winston](https://github.com/winstonjs/winston)

If you were using this library before version 2.0.0 have a look at the 
[migration guide for Winston](https://github.com/winstonjs/winston/blob/master/UPGRADE-3.0.md) and at the updated
[examples](examples).

 * [Features](#features)
 * [Installing](#installing)
 * [Configuring](#configuring)
 * [Usage](#usage)
 * [Options](#options)
 * [Examples](#examples)
 * [Simulation](#simulation)

### Features

 * logging to AWS CloudWatchLogs
 * [logging to multiple streams](#logging-to-multiple-streams)
 * [programmatically flush logs and exit](#programmatically-flush-logs-and-exit)
 * logging with multiple levels
 * creates group / stream if they don't exist
 * waits for an upload to suceed before trying the next
 * truncates messages that are too big
 * batches messages taking care of the AWS limit (you should use more streams if you hit this a lot)
 * support for Winston's uncaught exception handler
 * support for TypeScript, see [TypeScript definition](https://github.com/lazywithclass/winston-cloudwatch/blob/master/typescript/winston-cloudwatch.d.ts)
 * [see options for more](#options)

### Installing

```sh
$ npm install --save winston winston-cloudwatch
```

### Configuring

AWS configuration works using `~/.aws/credentials` as written in [AWS JavaScript SDK guide](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-configuring.html#Setting_AWS_Credentials).

As a best practice remember to use one stream per resource, so for example if you have 4 servers you should setup 4 streams
on AWS CloudWatch Logs, this is a general best practice to avoid incurring in token clashes and to avoid limits of the service (see [usage](#usage) for more).

#### Region note

As specified [in the docs](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/node-configuring.html#Setting_the_Region):

 > The AWS SDK for Node.js doesn't select the region by default.

so you should take care of that. See the [examples](#examples) below.

If either the group or the stream do not exist they will be created for you.

#### AWS UI

For displaying time in AWS CloudWatch UI you should click on the gear in the top right corner in the page with your logs and enable checkbox "Creation Time".

##### TypeScript

Remember to install types for both winston and this library.

### Usage

Please refer to [AWS CloudWatch Logs documentation](http://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutLogEvents.html) for possible contraints that might affect you.
Also have a look at [AWS CloudWatch Logs limits](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_limits.html).

In ES5
```js
var winston = require('winston'),
    WinstonCloudWatch = require('winston-cloudwatch');
```

```js
winston.add(new WinstonCloudWatch({
  logGroupName: 'testing',
}));

winston.error('<StreamName>', <Json Log>);
```
