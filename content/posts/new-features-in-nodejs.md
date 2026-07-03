+++
title = 'New Features in NodeJS'
date = 2026-07-03T00:00:00+05:30
lastmod = 2026-07-03T00:00:00+05:30
draft = false
tags = ['nodejs']
+++

## Loading Env files

Node.js can now load an env file either by specifying it in the command line (`--env-file`) or programmatically through `process.loadEnvFile` API.
<!--more-->
This now removes the need to use `dotenv` package for simple use cases.

Refer [this guide](https://nodejs.org/learn/command-line/how-to-read-environment-variables-from-nodejs) for more details.
