---
title: Belgic
weight: 3
sidebar:
  open: false
---

![](belgic.jpeg)

Belgic is a reverse proxy and load balancer that will ease
the deployment of an application on as well as improve
performances of said deployed application.

__Note that it also works for a shiny application__

It is to Ambiorix what shiny-server is to shiny.

{{% callout note %}}

This implements a round robin, requests are redirected to whatever backend is next on the queue. This means you should not store any session-related data in the environment, use databases, cookies, parameters, etc. (as one should anyway). This only applies to ambiorix (multi-page) and should not affect shiny applications (single-page).

{{% /callout %}}

## Next

{{< cards >}}
  {{< card url="/docs/belgic/install" title="Install" icon="document-duplicate" >}}
{{< /cards >}}
