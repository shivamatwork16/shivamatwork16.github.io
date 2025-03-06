+++
date = '2025-02-22T18:18:30+05:30'
draft = false
title = 'Scheduling Tasks with Node Cron: A Practical Guide'
tags = ["blog", "Nodejs"]
pin = true
+++

## Scheduling Tasks with Node Cron: A Practical Guide

<!--more-->

In Node.js applications, there are often scenarios where you need to schedule tasks to run at specific times or intervals. This is where the `node-cron` library comes in handy. It provides a simple and powerful way to schedule cron jobs within your Node.js applications.

### What is Cron?

Cron is a time-based job scheduler in Unix-like operating systems. It allows you to automate tasks by running them at specified times or intervals. Cron expressions are used to define these schedules.

### Installing Node Cron

To use `node-cron` in your Node.js project, you first need to install it:

```bash
npm install node-cron
```

### Basic Usage

Here's a basic example of how to schedule a task to run every minute:

```javascript
const cron = require("node-cron");

cron.schedule("* * * * *", () => {
  console.log("Running a task every minute");
});
```

The first argument to `cron.schedule` is the cron expression, and the second argument is the function that will be executed when the task runs.

### Cron Expressions

Cron expressions consist of five or six fields, representing:

- Minute (0-59)
- Hour (0-23)
- Day of the month (1-31)
- Month (1-12 or JAN-DEC)
- Day of the week (0-7 or SUN-SAT, where 0 and 7 are Sunday)

Here are some examples:

- `* * * * *`: Run every minute.
- `0 * * * *`: Run every hour at minute 0.
- `0 0 * * *`: Run every day at midnight.
- `0 0 1 * *`: Run on the first day of every month at midnight.
- `0 0 * * 0`: Run every Sunday at midnight.

### Scheduling Specific Tasks

You can schedule more complex tasks using specific values or ranges in your cron expressions:

```javascript
// Run at 10:30 AM on every 15th of the month
cron.schedule("30 10 15 * *", () => {
  console.log("Running a task at 10:30 AM on the 15th of every month");
});

// Run every 5 minutes
cron.schedule("*/5 * * * *", () => {
  console.log("Running a task every 5 minutes");
});
```

### Stopping a Task

You can stop a scheduled task by calling the `stop` method on the task object:

```javascript
const task = cron.schedule("* * * * *", () => {
  console.log("Running a task every minute");
});

// Stop the task
task.stop();
```

### Time Zones

By default, `node-cron` uses the server's local time zone. You can specify a different time zone using the `timezone` option:

```javascript
cron.schedule(
  "0 0 * * *",
  () => {
    console.log("Running a task at midnight in New York");
  },
  {
    scheduled: true,
    timezone: "America/New_York",
  },
);
```
