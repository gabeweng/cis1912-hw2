## Part 1

> What does a Deployment give you that a bare Pod doesn't? Why do we need a separate Service object instead of talking to a Pod's IP directly?

A deployment automates the creation, updating, and scaling of pods so even if a pod crashes, k8s will spin up another pod so that you have as many replicas as you declare. If you just try to create a pod, a pod crashes just stays down. A Service object is needed because each pod is assigned a new IP address every time it spins up, but a Service provides a stable IP address to use that doesn't depend on the underlying pods.

## Part 2

> What is a Helm chart, and how do `values.yaml` plus `-f`/`--set` let the same chart produce different Kubernetes resources for different environments? Why is this better than keeping separate copies of your Part 1 manifests for staging and production?

A Helm chart is a package of templated Kubernetes manifests and a `values.yaml` of default settings and a `Chart.yaml` of metadata. Installing a chart creates a release, a named and tracked instance of it. The templates contain placeholders like `{{ .Release.Name }}`, and Helm fills them in at install time to produce plain Kubernetes YAML. `values.yaml` supplies the defaults, and `-f values-staging.yaml` layers an override file on top of them, while `--set key=value` overrides individual fields on the command line, so the same templates render different resources (different model, CPU/memory limits, volume size) for each environment. This is better than keeping separate copies of the Part 1 manifests because there is a single source of truth, so a bug fix or structural change is made once in the templates and applies to every environment, and the environments differ only in a small values file, so they can't silently drift apart.
