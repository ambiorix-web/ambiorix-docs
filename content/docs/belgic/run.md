
---

title: Run
weight: 3
---

Once downloaded and configured the load balancer can be launched,
to do so simply:

```bash
./belgic start
```

## Daemonise

You would likely want to daemonise the process.

```bash
[Unit]
Description=Belgic

[Service]
ExecStart=path/to/belgic start
Restart=on-abnormal
Type=simple

[Install]
WantedBy=multi-user.target
```

Once the service is added restart the daemon, you might have to run it as `sudo`.

```bash
systemctl daemon-reload
```

You can then start and enable the service, again, you might have to run it as `sudo`.

```bash
systemctl start belgic
systemctl enable ambiorie
```

> [!WARNING]
> Currently, the load balancer does not restart when the app
> or any of the file change so after a deploy you _may have to_
> run `systemctl restart ambiorix`
