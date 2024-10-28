# ns8-portainer

Portainer is a web based container management.

## Install

Install via Software center:

Add a Software repository pointing to https://repo.mrmarkuz.com/ns8/updates/, check out the repo webpage how to do it.

Install via CLI:

    add-module ghcr.io/mrmarkuz/portainer:latest 1

The output of the command will return the instance name.
Output example:

    {"module_id": "portainer1", "image_name": "portainer", "image_url": "ghcr.io/mrmarkuz/portainer:latest"}

## Configure

Let's assume that the portainer instance is named `portainer1`.

Launch `configure-module`, by setting the following parameters:
- `<MODULE_PARAM1_NAME>`: <MODULE_PARAM1_DESCRIPTION>
- `<MODULE_PARAM2_NAME>`: <MODULE_PARAM2_DESCRIPTION>
- ...

Example:

    api-cli run module/portainer1/configure-module --data '{}'

The above command will:
- start and configure the portainer instance
- (describe configuration process)
- ...

Send a test HTTP request to the portainer backend service:

    curl http://127.0.0.1/portainer/

## Smarthost setting discovery

Some configuration settings, like the smarthost setup, are not part of the
`configure-module` action input: they are discovered by looking at some
Redis keys.  To ensure the module is always up-to-date with the
centralized [smarthost
setup](https://nethserver.github.io/ns8-core/core/smarthost/) every time
portainer starts, the command `bin/discover-smarthost` runs and refreshes
the `state/smarthost.env` file with fresh values from Redis.

Furthermore if smarthost setup is changed when portainer is already
running, the event handler `events/smarthost-changed/10reload_services`
restarts the main module service.

See also the `systemd/user/portainer.service` file.

This setting discovery is just an example to understand how the module is
expected to work: it can be rewritten or discarded completely.

## Uninstall

To uninstall the instance:

    remove-module --no-preserve portainer1

## Testing

Test the module using the `test-module.sh` script:


    ./test-module.sh <NODE_ADDR> ghcr.io/nethserver/portainer:latest

The tests are made using [Robot Framework](https://robotframework.org/)

## UI translation

Translated with [Weblate](https://hosted.weblate.org/projects/ns8/).

To setup the translation process:

- add [GitHub Weblate app](https://docs.weblate.org/en/latest/admin/continuous.html#github-setup) to your repository
- add your repository to [hosted.weblate.org]((https://hosted.weblate.org) or ask a NethServer developer to add it to ns8 Weblate project
