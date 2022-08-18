# Dokku orb

This orb provides the ability to deploy applications to dokku as part of your CI/CD pipelines.

[![CircleCI Build Status](https://circleci.com/gh/dokku/dokku-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/dokku/dokku-orb) ![Orb Version Badge](https://badges.circleci.com/orbs/dokku/dokku.svg) [![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/dokku/dokku-orb/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-lightblue.svg)](https://discuss.circleci.com/c/ecosystem/orbs)

## How to use

### Step 1 - Add a deploy key to CircleCI

The private key for the dokku user you wish to use for deploys will need to be added to CircleCI. See [the docs](https://circleci.com/docs/2.0/add-ssh-key/) for more on how to do this. The fingerprint for this key is required in step 2.

### Step 2 - Use the commands/jobs in your project as necessary

```yaml
---
version: 2.1
orbs:
  dokku: dokku/dokku@0.1.0
workflows:
  deploy:
    jobs:
      - checkout
      - add_ssh_keys:
          fingerprints:
            - "$SSH_KEY_FINGERPRINT"
      - dokku/deploy:
          git-remote-url: ssh://dokku@dokku.myhost.ca:22/appname
```

See more examples on the [CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/dokku/dokku).

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/dokku/dokku) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/dokku/dokku-orb/issues) to and [pull requests](https://github.com/dokku/dokku-orb/pulls) against this repository!
