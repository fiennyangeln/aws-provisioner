TaskCluster AWS Provisioner
============================
This repository contains a EC2 instance provisioner for a TaskCluster instance.
For the time being it has a focus on submitting and canceling spot instance
requests, as well as terminating instance should be create too many.
This server basically monitors the number of pending tasks, running instances
and pending spot requests, then submits, cancels or terminates AWS resources
according to the configuration. Initial implementation is quite naive.

Quick Start
-----------
  1. Install dependencies `npm install`,
  2. Configure AWS with `node utils/setup-aws.js`, this will create a local
     config file.
  2. Configure the server (see `config.js`),
  3. Run the server with `node server.js`
  4. Cleanup AWS with `node utils/cleanup-aws.js`.


AWS Configuration and Clean-up
------------------------------
This provisioner relies on a special key-name to find and identify spot requests
and instances managed by this provisioner. Consequently, if other processes or
**people** creates spot request or EC2 instances with this special key-name,
then behavior is undefined.

The special key-name is configured in any config-file read by `nconf`, see
`config.js` for where these can be put. The utility `utils/setup-aws.js` will
create a unique key-name and save it in a local config file. The utility
`utils/cleanup-aws.js` will delete all AWS resources associated with the
key-name and delete the key-name from local config file.

For testing purposes, run `utils/setup-aws.js` to create a key-name and when
testing is done clean up with `utils/cleanup-aws.js`. In production, it's
recommended that a special key-name in constructed and configured manually.