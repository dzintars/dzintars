---
title: "How to Deploy Rootless HAProxy as Podman Quadlet"
date: "2024-09-03T16:24:34+03:00"
draft: false
tags: ["HAProxy", "Podman", "Linux", "Systemd"]
categories: ["How To's"]
---

{{< badge label="Linux" icon="linux" color="FCC624" alt="Linux Badge" >}}
{{< badge label="Podman" icon="podman" color="892CA0" alt="Podman Badge" >}}

{{< note title="This post is raw. No grammar is checked." >}}

When doing local development or just tinkering with this or that, quite often
you want to expose something to the Internet.

When it's just single "something", then you can just forward your port 80 from
router, to your machine's "something" port.

But this falls appart if you have two "something" you want to expose to the
Internet. You can't forward single `80` port from your router to two different
applications running on your machine. Because one application will be running
for example on `8080` port and the other on `8081`.

To solve this, you need **Revese Proxy**. Not the Forward Proxy. That is different
thing often used in corporate networks to limit and secure outgoing employee
traffic.

So... Reverse Proxy.

There are many solutions you can pick from. Nginx (which can act as reverse proxy,
but in it's core it is just pure WEB server), Traefik, Envoy and others.

But today I will talk about HAProxy. Really cappable Layer 4 proxy.

I will not go deep in the weeds about it in this post.

I just want to show, how to run it locally on your machine as root-less Podman
Quadlet.

Personally I am using Ansible templates to render all the files and resources,
but in this post I will show just **MINIMAL** working files which you need to get it
running.

I will not tell, how to configure TLS certificates. And I will not
explain all in's and out's of HAProxy configuration.

But we will enable **Dataplane API** so you can dynamically tweak the configuration
of your HAProxy instance.

We need to create 3 files in `~/.config/containers/systemd` directory.

- `haproxy-configmap.yaml`
- `haproxy.kube`
- `haproxy.yaml`

Let's go with the `haproxy-configmap.yaml` first:

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: haproxy-cfg
data:
  haproxy.cfg: |
    program api
      command /usr/local/bin/dataplaneapi -f /usr/local/etc/haproxy/dataplaneapi.yaml
      no option start-on-reload

    global
      maxconn         20000
      ulimit-n        16384
      log             127.0.0.1 local0
      daemon
      stats socket /usr/local/run/haproxy/api.sock user haproxy group haproxy mode 660 level admin expose-fd listeners

  dataplaneapi.yaml: |
    dataplaneapi:
      host: 0.0.0.0
      port: 5555
      transaction:
        transaction_dir: /tmp/haproxy
      user:
        - insecure: true
          name: admin
          password: adminpass
    haproxy:
      config_file: /usr/local/etc/haproxy/haproxy.cfg
      haproxy_bin: /usr/sbin/haproxy
      reload:
        reload_delay: 5
        reload_cmd: "kill -SIGUSR2 1"
        restart_cmd: "kill -SIGUSR2 1"
```

This is our minimal HAProxy configurations to get it running.

We have defined 2 sets of configurations which will be mounted as files in the
container. First one is `haproxy.cfg` and the second one is `dataplaneapi.yaml`.

Dataplane and Haproxy will run as 2 separate processes within the single
container.

Dataplane also can be configured in HashiCorp Configuration Language `HCL`, but
we will use `yaml` there.

Next, we need to create Systemd unit file `haproxy.kube`. It will be used by
Quadlet generator to generate proper Systemd unit.

```toml
[Install]
WantedBy=default.target

[Unit]
Description=HAProxy

[Kube]
Yaml=haproxy.yaml
UserNS=keep-id:uid=1000,gid=1000

PublishPort=8080:8080
PublishPort=8443:8443
PublishPort=5555:5555
PublishPort=8404:8404

ConfigMap=haproxy-configmap.yaml

```

As you can see, we use unpriviledged ports. And we point to the 2 other files
there.

Next goes our almost last file - `haproxy.yaml` which is just a Podman Pod
manifest.

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: haproxy
  annotations:
    io.podman.annotations.infra.name: haproxy

spec:
  hostname: haproxy
  containers:
    - name: server
      image: localhost/your-namespace/haproxy:2.9.7-rootless
      imagePullPolicy: IfNotPresent
      workingDir: /usr/local/etc/haproxy
      securityContext:
        runAsGroup: 1000
        runAsUser: 1000
        fsGroup: 1000
        allowPrivilegeEscalation: false
        capabilities:
          drop:
            - ALL
      resources:
        limits:
          memory: 512Mi
        requests:
          memory: 512Mi
      ports:
        - name: http
          containerPort: 8080
          hostPort: 8080
          protocol: TCP
        - name: https
          containerPort: 8443
          hostPort: 8443
          protocol: TCP
        - name: dapi
          containerPort: 5555
          hostPort: 5555
          protocol: TCP
        - name: stats
          containerPort: 8404
          hostPort: 8404
          protocol: TCP
      volumeMounts:
        - name: haproxy-cm
          mountPath: /usr/local/etc/haproxy:rw

  volumes:
    - name: haproxy-cm
      configMap:
        name: "haproxy-cfg"
        items:
          - key: haproxy.cfg
            path: haproxy.cfg
          - key: dataplaneapi.yaml
            path: dataplaneapi.yaml
```

Nothing too fancy there. We are running container as `1000` user which is
`haproxy`. We drop all container capabilities. And we mount our configmap items
as files there.

But there are single issue which you might noticed.

We are using some kind of `localhost/your-namespace/haproxy:2.9.7-rootless` image.

Well... yeah... official HAProxy image is not tailored to run as root-less. So
we need to build our own custom image to tweak some little things.

Don't panic. It's simple.

We need to create simple `Containerfile`:

```yaml
FROM 'docker.io/haproxytech/haproxy-debian:2.9.7'

STOPSIGNAL SIGTERM

RUN set -eux && \
mkdir -p /usr/local/run/haproxy /usr/local/etc/haproxy && \
chown -R haproxy:haproxy /usr/local/run/haproxy /usr/local/etc/haproxy
```

That's it.

`your-namespace` can be whatever you want. Use your username if you are not
sure. And of course you can bump the HAProxy version.

Now, within the directory where your `Containerfile` is located execute: `podman
build -t localhost/your-namespace/haproxy:2.9.7-rootless`

Podman will pick your `Containerfile` automatically. But you can pass the
location of the Containerfile by using `-f path/to/Containerfile`.

As you can see, in our custom image we simply ensure that config directories are
presented and we set the permissions so that `haproxy` user in the container can
access these directories.

Use `podman image ls | grep haproxy` to see, if the image is there.

So, once our image is built, we can deploy our Podman Quadlet.

Simply run `systemctl --user daemon-reload` for the Systemd to pick up our 3 new
config/unit files and then `systemctl --user start haproxy.service`.

This should start the HAProxy Pod.

`podman pod ps` should tell you that haproxy is running.

Basically, that's it!

HAProxy should be functional, but not very usefull as it basically has no
configuration.

To do the configuration... you have few options.

- You can extend our ConfigMap with the config sections you want.
- You can use `curl` and Dataplane API to "inject" the configs dynamically.
- You can use Terraform and Dataplane API. Or Ansible.

I would recommend to use just Dataplane API and at least simple Bash scripts
with bunch of `curl` commands to do the config. Don't create manual edits in the
config. Even more - don't create manual edits in the container itself.

To interact with Dataplane API use:

```bash
curl -X GET   --user admin:pass   "http://127.0.0.1:5555/v2/services/haproxy/configuration/global" | jq
```

There we are "curling" into local container over local `127.0.0.1` IP and then just
using regular REST endpoints to GET or POST the config fragments as JSON.

```bash
curl -X POST \
  --user admin:pass \
  -H "Content-Type: application/json" \
  -d '{
      "name": "my_backend",
      "mode":"http",
      "balance": {
          "algorithm":"roundrobin"
       },
       "default_server": {
           "alpn": "h2",
           "check": "enabled",
           "check_alpn": "h2",
           "maxconn": 30,
           "weight": 100
        }
    }' \
  "http://127.0.0.1:5555/v2/services/haproxy/configuration/backends?version=1"
```

For example, there we are creating "skeleton" for some backend.

Pay attention to the `?version=1` at the end of the query. Dataplane API will
automatically increase the version number of the `haproxy.cfg` file every time
you make some changes. It is so that you can roll back or to protect against
concurent config updates if some of the other team members do the updates to
the config at the same time as you do.

So... in essence... that's it. You should have functional HAProxy.

Forward your router's `80` and `443` port to your HAProxy's host `8080` and `8443` ports.

As somebody will hit your domain and will get redirected to your home's static
IP, your router will redirect that request to the `8080` or `8443` port. What
happens from there... now is up to your HAProxy's config.

To stop the HAProxy use `systemctl --user stop haproxy.service`.
To get into container use `podman exec -it haproxy-server /bin/bash`
To get into container as root user use `podman exec -u 0 -it haproxy-server /bin/bash`
To see the logs `journalctl --user -xeu haproxy.service -f`

Things to improve:

- Systemd lingering. So that Pod starts as you turn on your machine and uses
  dedicated user for that.
- TLS for the Dataplane API
- Securing Dataplane API and Stats user/s credentials
- Automated setup via Ansible
- Forward all **http** traffic to **https**
- Enable Let'sEncrypt ACME path rule (so that you can use Certbot without DNS
  method)
- Use `crt-list` to combine different TLS certificates
- SSH handling. In case if you run your own Git server, you might want to proxy
  to multiple SSH targets.

So... before going deeper into the weeds... make sure you have minimal working
example to which you can always return when things goes south.
