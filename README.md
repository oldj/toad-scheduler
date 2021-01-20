# toad-scheduler

[![Build Status](https://github.com/kibertoad/toad-scheduler/workflows/ci/badge.svg)](https://github.com/kibertoad/toad-scheduler/actions)
[![Coverage Status](https://coveralls.io/repos/kibertoad/toad-scheduler/badge.svg?branch=master)](https://coveralls.io/r/kibertoad/toad-scheduler?branch=master)

In-memory Node.js job scheduler

## Getting started

First install the package:

```bash
npm i toad-scheduler
```

Next, set up your jobs:

```js
const { ToadScheduler, SimpleIntervalJob, Task } = require('toad-scheduler')

const scheduler = new ToadScheduler()

const task = new Task('simple task', () => { counter++ })
const job = new SimpleIntervalJob({ seconds: 20, }, task)

scheduler.addSimpleIntervalJob(job)
```

## Usage with async tasks

In order to avoid unhandled rejections, make sure to use AsyncTask if you task is asynchronous:

```js
const { ToadScheduler, SimpleIntervalJob, AsyncTask } = require('toad-scheduler')

const scheduler = new ToadScheduler()

const task = new AsyncTask(
    'simple task', 
    () => { return db.pollForSomeData().then((result) => { /* continue the promise chain */ }) },
    (err: Error) => { /* handle error here */ }
)
const job = new SimpleIntervalJob({ seconds: 20, }, task)

scheduler.addSimpleIntervalJob(job)
```

Note that in order to avoid memory leaks, it is recommended to use promise chains instead of async/await inside task definition.
