---
id: app_help
title: Customizing Application's help
sidebar_label: Customizing Application's help
---
Hydra provides two different help options:
* `--help` : Application specific help
* `--hydra-help` Hydra specific help. 

Example output of `--help`:
```text
$ python my_app.py --help
my_app is powered by Hydra.

== Configuration groups ==
Compose your configuration from those groups (group=option)

db: mysql, postgresql


== Config ==
Override anything in the config (foo.bar=value)

db:
  driver: mysql
  pass: secret
  user: omry


Powered by Hydra (https://cli.dev)
Use --hydra-help to view Hydra specific help
```

This output is generated from the following default configuration.
You can override the individual components like `hydra.help.app_name` or the whole template. 
```yaml
hydra:
  help:
    # App name, override to match the name your app is known by
    app_name: ${hydra.job.name}

    # Help header, customize to describe your app to your users
    header: |
      ${hydra.help.app_name} is powered by Hydra.

    footer: |
      Powered by Hydra (https://cli.dev)
      Use --hydra-help to view Hydra specific help

    # Basic Hydra flags:
    #   $FLAGS_HELP
    #
    # Config groups, choose one of:
    #   $APP_CONFIG_GROUPS: All config groups that does not start with hydra/.
    #   $HYDRA_CONFIG_GROUPS: All the Hydra config groups (starts with hydra/)
    #
    # Configuration generated with overrides:
    #   $CONFIG : Generated config
    #
    template: |
      ${hydra.help.header}
      == Configuration groups ==
      Compose your configuration from those groups (group=option)

      $APP_CONFIG_GROUPS

      == Config ==
      Override anything in the config (foo.bar=value)

      $CONFIG

      ${hydra.help.footer}
```