# OliveTin (Helm Chart)

[![Maturity Badge](https://img.shields.io/badge/maturity-Production-brightgreen)](#none)
[![Discord](https://img.shields.io/discord/846737624960860180?label=Discord%20Server)](https://discord.gg/jhYWWpNJ3v)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/olivetin)](https://artifacthub.io/packages/search?repo=olivetin)

Give safe and simple access to predefined shell commands from a web interface.

[Instructions for using the OliveTin Helm chart](https://docs.olivetin.app/install/helm.html)

## Quickstart

    user@host: helm repo add olivetin https://olivetin.github.io/OliveTin-HelmChart/
    user@host: helm install olivetin olivetin/olivetin 
    
## Default values file;

```yaml
replicaCount: 1

image:
  repository: ghcr.io/olivetin/olivetin
  pullPolicy: IfNotPresent
  tag: ""

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  create: true
  automount: true
  annotations: {}
  name: ""

podAnnotations: {}
podLabels: {}

podSecurityContext: {}
  # fsGroup: 2000

securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

service:
  type: ClusterIP
  port: 1337

ingress:
  enabled: false
  className: ""
  annotations: {}
    # kubernetes.io/ingress.class: nginx
    # kubernetes.io/tls-acme: "true"
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
  #  - secretName: chart-example-tls
  #    hosts:
  #      - chart-example.local

resources: {}
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
  #   memory: 128Mi

# This is to setup the liveness and readiness probes more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/
livenessProbe:
  initialDelaySeconds: 5
  periodSeconds: 20
  httpGet:
    path: /api/readyz
    port: http
readinessProbe:
  periodSeconds: 20
  httpGet:
    path: /api/readyz
    port: http

# Additional volumes on the output Deployment definition.
extraVolumes: []
# - name: foo
#   secret:
#     secretName: mysecret
#     optional: false

# Additional volumeMounts on the output Deployment definition.
extraVolumeMounts: []
# - name: foo
#   mountPath: "/etc/foo"
#   readOnly: true

nodeSelector: {}

tolerations: []

affinity: {}

config: {}

olivetin:
  config:
    rawYaml:
      actions:
        - title: "Hello world!"
          shell: echo 'Hello World!'

# -- Extra k8s manifests to deploy
# Might be useful for adding simple scripts to OliveTin container
# without rebuilding base image or custom themes
extraObjects: []
# - apiVersion: v1
#   kind: ConfigMap
#   metadata:
#     name: olivetin-shell-scripts
#   data:
#     test-script.sh: |
#       #!/usr/bin/env sh
#       echo "I am a test script"
```
