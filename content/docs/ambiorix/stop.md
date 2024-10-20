---
title: Start & Stop
weight: 2
---

You can check whether the app is running.

```r
# is server running
app$is_running
```

You can start the server with.

#### CLI

```bash
ambiorix-cli start
```

#### R

```r
# stop all servers
app$start()
```

You can stop all servers with.

#### CLI

```bash
ambiorix-cli stop
```

#### R

```r
# stop all servers
stop_all()
```

By default ambiorix stops the server when the `start` method closes, setting `auto_close` prevents that, the severs can then be stopped with the `stop` method.

```r
# start
app$start(auto_stop = FALSE)

app$stop()
```

## On stop

One can also pass a callback to run when the server closes, this is ideal to do things like closing database connections.

```r
# start
app$on_stop <- \(){
  cat("Bye!\n")
}
```
