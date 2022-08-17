# Dokku orb

This orb provides the ability to deploy applications to dokku as part of your CI/CD pipelines.

[![CircleCI Build Status](https://circleci.com/gh/dokku/dokku-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/dokku/dokku-orb) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/dokku/dokku)](https://circleci.com/orbs/registry/orb/dokku/dokku) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/dokku/dokku-orb/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)

## How to use

### Step 1 - Add a deploy key to CircleCI

The private key for the dokku user you wish to use for deploys will need to be added to CircleCI. See [the docs](https://circleci.com/docs/2.0/add-ssh-key/) for more on how to do this. The fingerprint for this key is required in step 2.

### Step 2 - Use the commands/jobs in your project as necessary

```yaml
---
version: 2.1

orbs:
  dokku: dokku/dokku@0.3.0

workflows:
  build-test-deploy:
    jobs:
      - dokku/git-deploy:
          git-remote-url: ssh://dokku@dokku.myhost.ca:22/appname
```

See further examples and documentation can be found on the [CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/dokku/dokku).

### IP whitelisting

Your dokku host may be behind a firewall, in which case you might want to whitelist the IP of the builder prior to using the commands exposed through this orb. Though this orb does not include any commands or jobs specific to IP whitelisting, this can be accomplished using other third party orbs. Consider the example below for inspiration.

```yaml
---
version: 2.1

orbs:
  dokku: dokku/dokku@0.3.0
  ip-address-helper: dokku/ip-address-helper@1.0.0

jobs:
  deploy-behind-firewall:
    executor: dokku/default
    resource_class: small
    steps:
      - checkout
      # See implementation of this orb - it is just a convenient way of grabbing the externally facing IP address of the builder
      - ip-address-helper/set-env-var
      - run:
          name: Whitelist the IP
          command: |
            # TODO use an API to whitelist IP - e.g AWS, DigitalOcean, Scaleway etc have API's for adding the IP to a security group.

      - add_ssh_keys:
          fingerprints:
            - "SO:ME:FIN:G:ER:PR:IN:T"

      - dokku/git-deploy:
          git-remote-url: ssh://dokku@dokku.myhost.ca:22/appname
        
      - run:
          name: Remove the IP
          command: |
            # TODO use an API to remove the IP
          when: always # this line ensures this will still be run even if the job fails for some reason

workflows:
  deploy:
    jobs:
      - deploy
```

**Note:** Several caveats exist with the above approach, including but not limited to the fact that docker jobs are not likely to have exclusive use of an external IP address and in the unlikely case of infrastructure failure, an IP could fail to be removed from your whitelist. If security is paramount [CircleCI's Runner](https://circleci.com/docs/2.0/runner-overview/#introduction) is likely a better a fit.

## Resources

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/dokku/dokku) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/dokku/dokku-orb/issues) to and [pull requests](https://github.com/dokku/dokku-orb/pulls) against this repository!
