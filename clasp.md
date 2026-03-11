# Using clasp to deploy GAS-ICS-Sync

This is a fork of Derek Antrican's
[GAS-ICS-Sync](https://github.com/derekantrican/GAS-ICS-Sync),
which installs a script into
Google Apps Scripts that synchronised iCal / ICS sources into a Google calendar.
This repo separates out the settings into a separate settings.js file that is
not added to git, so that multiple versions of GAS-ICS-Sync can be deployed for
different syncs using
[clasp](https://developers.google.com/apps-script/guides/clasp).

## Install npm and clasp

```shell
sudo apt install -y npm
sudo npm install -g @google/clasp
```

## Login

```shell
clasp login
```

This creates local HTTP server to receive the callback from Google
authentication so if your default browser is not Chrome, it may be best to
paste the URL the command prints out into Chrome.

## Copy settings

```shell
cp settings.js.sample settings.js
```

Edit the `settings.js` file and add details for your calendars.

## Create a Google Apps Script project

```shell
clasp create --type api --title "ical-sync"
```

## Push the project

```shell
clasp push
```

## Add a trigger

TBA
