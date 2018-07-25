---
title: OpenShift
description: Instructions to setup an OpenShift cluster for Istio.
weight: 18
keywords: [platform-setup,openshift]
---

To setup an OpenShift cluster for Istio, follow these instructions:

By default, OpenShift doesn't allow containers running with user ID 0.

Enable containers running with UID 0 for Istio's service accounts:

    {{< text bash >}}
    $ oc adm policy add-scc-to-user anyuid -z istio-ingress-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z default -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z prometheus -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-egressgateway-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-citadel-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-ingressgateway-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-cleanup-old-ca-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-mixer-post-install-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-mixer-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-pilot-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-sidecar-injector-service-account -n istio-system
    $ oc adm policy add-scc-to-user anyuid -z istio-galley-service-account -n istio-system
    {{< /text >}}

The list above accounts for the default Istio service accounts. If you enabled
other Istio services, like _Grafana_ for example, you need to enable its
service account with a similar command.

A service account that runs application pods needs privileged security context
constraints as part of sidecar injection.

    {{< text bash >}}
    $ oc adm policy add-scc-to-user privileged -z default -n <target-namespace>
    {{< /text >}}

> Check for `SELINUX` in this [discussion](https://github.com/istio/issues/issues/34)
> with respect to Istio in case you see issues bringing up the Envoy.