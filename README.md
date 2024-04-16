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

## Uninstall

To uninstall the instance:

    remove-module --no-preserve kimai1
