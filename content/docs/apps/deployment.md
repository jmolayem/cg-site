---
menu:
  docs:
    parent: apps
title: Basic deployment tips
weight: -100
---

## How deployment works

The command to create a new app and to push a new version of an existing one are the same: `cf push`. The basic steps:

1. Check out whatever version of the code you want to deploy.

    ```bash
    git checkout master
    ```

1. [Target]({{< relref "docs/getting-started/concepts.md#target" >}}) the appropriate [organization]({{< relref "docs/getting-started/concepts.md#organizations" >}})/[space]({{< relref "docs/getting-started/concepts.md#spaces" >}}).

    ```bash
    cf target -o <SOMEORG> -s <SOMESPACE>
    ```
1. Deploy the application.

    ```bash
    cf push <APPNAME>
    ```

The app should now be live at `APPNAME.app.cloud.gov`.

## Caveats

* Don't write to local storage (it's ephemeral) – use S3 [service]({{< relref "docs/apps/managed-services.md" >}}) instead
* Instances will be restarted if they exceed [memory limits]({{< relref "docs/apps/limits.md" >}})
* Proper [logging]({{< relref "docs/apps/logs.md" >}}) might require special libraries/configuration for your app

## Twelve-factor apps

In general, applications will be easiest to deploy to Cloud Foundry if they follow the [Twelve Factor App](http://12factor.net/) guidelines.

## Setting environment variables

See Cloud Foundry's [documentation on environment variables](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html).

## Exclude files

Cloud Foundry isn't version-control-aware, so `cf push` will deploy the working state of whatever files you have in that directory. In most cases, you will want to [exclude files](http://docs.cloudfoundry.org/devguide/deploy-apps/prepare-to-deploy.html#exclude) ignored by Git. From within your project directory, run

```bash
ln -s .gitignore .cfignore
```

and commit the `.cfignore` to your repository. However, read on if you have a more advanced CF setup.

A couple of important points on the `.cfignore`:

1. if you have a more advanced app setup and have apps with a `path` other than the project root (where you run `cf push` from), you will need an additional `.cfignore` file located in each app `path`;
2. also note that more advanced `.gitignore` syntax, such as the `**` recursive subdirectory wildcard, are _not_ supported by `.cfignore`.