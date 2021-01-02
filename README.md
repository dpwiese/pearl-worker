# PEARL Worker

![node-ci](https://github.com/dpwiese/pearl-worker/workflows/node-ci/badge.svg)

This repository contains a simple node worker that can be triggered by a cron job periodically to fetch and process some data on an FTP server, and upload the output as JSON to S3.
Other than the minimal dependencies needed below, the only other requirement to run this code is an S3 bucket.

## Setup

The following instructions are written for Mac, but should work on Linux.
First install [nvm](http://nvm.sh) with the following command:

```sh
% curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

Then install LTS [node](https://nodejs.org/en/)

```sh
nvm install --lts
```

Navigate to the root project directory and install dependencies

```sh
npm install
```

Finally, create your `.env` file in the project root directory duplicating `.env.sample`, but with your own credentials.
You should now be ready to run and develop! 🏁

## Using

To compile the TypeScript source run

```sh
% tsc
```

This will generate the output `worker.js` which you can execute with the following command

```sh
% node worker.js
```

## Editing

Before commiting any edits make sure the project compiles and then run the lint command to fix any formatting errors

```sh
% npm run lint
```

# Scheduling

This worker is currently running as a cron job on EC2, scheduled at the top of every hour.
This cronjob can be equivalently scheduled on your local machine with `crontab -e` and entering the following, adapted to your local environment.

```
# Pull and process PEARL data every hour on the hour
0 * * * * root cd /path/to/pearl-worker && /path/to/node ./worker.js > /your/output/dir/cron.log
```
