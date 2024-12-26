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

You can login as user `admin` with password `Nethesis,1234`
You may want to change the password and the example mail address in the profile.

## Plugins

In the following examples we're working with the [DeductionTimeBundle plugin](https://www.kimai.org/store/keleo-deduction-time-bundle.html)
Other plugins work in a similar way.
Please also adapt the kimai instance name in the following commands, in my case it was kimai1.

### Installation

Enter the kimai instance environment, in this example kimai1:

    runagent -m kimai1

Download the plugin, you can get the download Link from the plugins webpage:

    curl -L -O https://github.com/Keleo/DeductionTimeBundle/archive/refs/tags/2.0.1.tar.gz

Extract the plugin:

    tar -xvf 2.0.1.tar.gz

Rename the extracted plugin directory to avoid version ending ("-2.0.1"):

    mv DeductionTimeBundle-2.0.1 DeductionTimeBundle

Copy the plugin to the container:

    podman cp DeductionTimeBundle kimai-app:/opt/kimai/var/plugins/

Set correct owner using chown in the container:

    podman exec kimai-app chown -R www-data: /opt/kimai/var/plugins/DeductionTimeBundle

Reload cache to make the new plugin visible:

    podman exec kimai-app /opt/kimai/bin/console kimai:reload --env=prod

The result should look like this, the depreciation warnings can be ignored.

```
 [OK] Cache for the "prod" environment (debug=false) was successfully warmed.   
 [OK] Kimai config was reloaded
```

Clean up - delete downloads:

    rm -rf DeductionTimeBundle 2.0.1.tar.gz

Exit the environment:
    
    exit

The plugin should now be visible in the web UI.

NOTE: The owner IDs may change after updates, restores or reinstalls so please check if the owner is the right one:

    ls -lisa /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins

As root, change to the data dir.

    cd /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins/_data

Change owner to the right one:

    chown -R --reference=. DeductionTimeBundle

### Remove

To remove a plugin just remove it's directory and reload the cache.

Delete the plugin directory:

    rm -rf /home/kimai1/.local/share/containers/storage/volumes/kimai-plugins/_data/DeductionTimeBundle

Reload cache:
    
    runagent -m kimai1 podman exec kimai-app /opt/kimai/bin/console kimai:reload --env=prod

### Update

To update a plugin, just remove it and reinstall it as explained above.

## Uninstall Kimai

To uninstall the instance:

    remove-module --no-preserve kimai1
