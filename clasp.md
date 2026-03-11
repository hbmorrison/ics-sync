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
cp settings.gs.sample settings.gs
```

Edit the `settings.gs` file and add details for your calendars.

## Create a Google Apps Script project

```shell
clasp create --title "<something>-ical-sync"
```

Once the project has been created, an `appsscript.json` manifest file will have
been created by `clasp`. Copy the `appsscript.json.sample` file over it so that
the project will request access to the correct Google APIs.

## Push the project

```shell
clasp push
```

When prompted, confirm that you wish to push and overwrite the manifest file
that clasp has created.

## Add a trigger

Go to [Google Apps Scripts](https://script.google.com/home), and click on the new
project that `clasp` has created. Ensure that `Code.gs` is selected on the left and
check that the `install` function is selected, which can be found at the top of the
page, next to Run and Debug, then click Run.

Google will pop up a "Google hasn’t verified this app" warning. Click the grey
`advanced` link at the bottom of the pop up, then click the grey `Go to
<something>-ics-sync (unsafe)` link that appears further down.

Tick the `Select all` checkbox on the permissions page that appears next, then
click the `Continue` button at the bottom.

Select the Triggers tab on the left. There will now be three triggers: a
`checkForUpdate` trigger that runs nightly at 1am, a `startSync` trigger that
will run once in a few minutes after the `install` function was run, before
being disabled, and a permanent `startSync` trigger that will run every fifteen
minutes (or whatever you have specified in `settings.gs`).

Select the Executions tab to see log output from individual runs.
