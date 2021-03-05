# Dokku orb

This orb provides the ability to deploy applications to dokku as part of your CI/CD pipelines.

[![CircleCI Build Status](https://circleci.com/gh/aaronstillwell/dokku-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/aaronstillwell/dokku-orb) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/aaronstillwell/dokku)](https://circleci.com/orbs/registry/orb/aaronstillwell/dokku) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/aaronstillwell/dokku-orb/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)

## How to use

```
version: 2.1

orbs:
  dokku: aaronstillwell/dokku@0.1.1

workflows:
  build-test-deploy:
    jobs:
      - dokku/git-deploy:
          app-name: my-app
          host: dokku.my-domain.io
```

See further examples and documentation can be found on the [CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/aaronstillwell/dokku).

### IP whitelisting

Your dokku host may be behind a firewall, in which case you might want to whitelist the IP of the builder prior to using the commands exposed through this orb. Though this orb does not include any commands or jobs specific to IP whitelisting, this can be accomplished using other third party orbs. Consider the example below for inspiration.

```
version: 2.1

orbs:
  dokku: aaronstillwell/dokku@0.1.1
  ip-address-helper: aaronstillwell/ip-address-helper@1.0.0

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

      # Now interface with dokku as normally over SSH
      - dokku/git-deploy:
          host: my-dokku-host.io
          app-name: my-dokku-app
        
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

[CircleCI Orb Registry Page](https://circleci.com/orbs/registry/orb/aaronstillwell/dokku) - The official registry page of this orb for all versions, executors, commands, and jobs described.
[CircleCI Orb Docs](https://circleci.com/docs/2.0/orb-intro/#section=configuration) - Docs for using and creating CircleCI Orbs.

### How to Contribute

We welcome [issues](https://github.com/aaronstillwell/dokku-orb/issues) to and [pull requests](https://github.com/aaronstillwell/dokku-orb/pulls) against this repository!

