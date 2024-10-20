
---

title: Configuration
weight: 2
---

Belgic requires a basic configuration file to run.

## Create

You can create the default config file with:

```bash
./belgic config -p=path/to/config.json
```

You can, of course, create it manually, it's a very
simple file anyway.

## File

The config file looks like:

```json
{
 "path": "/belgic",
 "port": "8080",
 "backends": "max",
 "attempts": 3
}
```

- `path`: the path containing the ambiorix application
you want to serve. It assumes the application is in an `app.R` file.
- `port`: port on which the apps should be served.
- `backends`: number of background applications to run in the background.
Defaults to the maximum number of cores available on the machine.
- `attempts`: number of times to try and revive a backend if it dies.

## Environment Variable

Add the `BELGIC_CONFIG` environment variable to point to the configuration
file you just created.

```bash
BELGIC_CONFIG="path/to/belgic.json"
```
