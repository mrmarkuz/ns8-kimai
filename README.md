# ns8-kimai

[Kimai](https://www.kimai.org/) is an open source time tracking software.

## Install

Install from Software center:

  - Add a Software repository pointing to `https://repo.mrmarkuz.com/ns8/updates/`, see https://repo.mrmarkuz.com for instructions
  - Install Kimai via Software Center

...or install from CLI:

    add-module ghcr.io/mrmarkuz/kimai:latest 1

## Configure

Configure at least the hostname (FQDN) in the app settings.

You can login as user `superadmin` with password `Nethesis,1234`
You may want to change the password and the example mail address in the profile.

## Plugins

In the following examples we're working with the [DeductionTimeBundle plugin](https://www.kimai.org/store/keleo-deduction-time-bundle.html)
Other plugins work in a similar way.
Please also adapt the kimai instance name in the following commands, in my case it was kimai1.

### Installation

Download the plugin, you can get the download Link from the plugins webpage:

    curl -L -O https://github.com/Keleo/DeductionTimeBundle/archive/refs/tags/2.0.1.tar.gz

Extract the plugin:

    tar -xvf 2.0.1.tar.gz

Move the extracted plugin directory to the Kimai plugins volume without the version ending ("-2.0.1"):

    mv DeductionTimeBundle-2.0.1/ /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins/_data/DeductionTimeBundle

Set correct owner:

    chown -R 558784:558784 /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins/_data/DeductionTimeBundle/

Reload cache to make the new plugin visible:

    runagent -m kimai1 podman exec kimai-app /opt/kimai/bin/console kimai:reload --env=prod

The plugin should now be visible in the web UI.

### Remove

To remove a plugin just remove it's directory and reload the cache.

Delete the plugin directory:

    rm -rf /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins/_data/DeductionTimeBundle

Reload cache:
    
    runagent -m kimai1 podman exec kimai-app /opt/kimai/bin/console kimai:reload --env=prod

### Update

To update a plugin, just remove it and reinstall it as explained above.

## Uninstall

To uninstall the instance:

    remove-module --no-preserve kimai1
