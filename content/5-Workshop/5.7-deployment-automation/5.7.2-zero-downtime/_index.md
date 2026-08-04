---
title: "Zero-Downtime Deployment"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---


The `--force-new-deployment` flag in the `deploy.ps1` script triggers a highly sophisticated AWS ECS mechanism, commonly known as **Rolling Update**.

Thanks to this mechanism, your system experiences absolutely no downtime while updating to a new version. The process unfolds as follows:

1. **Initialize new Tasks:** ECS keeps the old Tasks (running old code) intact. Simultaneously, it pulls the new image from ECR and creates new Tasks.
2. **Health Check:** The new Tasks start booting up (running `dotnet run`). The ALB will continuously ping their port 8080.
3. **Traffic Routing:** Only when the ALB confirms the new Tasks are healthy (Healthy 200 OK) does it start routing (Forwarding) user requests to them.
4. **Draining old Tasks:** ALB stops sending new requests to the old Tasks, waiting for them to finish processing ongoing requests. Finally, ECS issues a command to terminate (SIGTERM) the old Tasks.

> [!NOTE]
> The result is: Users currently using the App will not even know the system was just updated, no requests are dropped (502 Bad Gateway), ensuring a 100% seamless experience.
