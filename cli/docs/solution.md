# Solution YAML

This file describes the yaml spec for a solution that are deployed with arete.

```
apiVersion: arete/v1alpha1                                      # version of the configuration
kind: Config                                                    # always `Config`
metadata:                                                       # hold additional information about the config
  name:                                                         # solution name
  annotations:                                                  #
    config.kubernetes.io/local-config: "true"                   # apply the local config flag so that this file won't be applied to the cluster
spec:
  url: "https://github.com/user/repo"                           # the github url of the solution
  description: "Cool solution"                                  # short description of the solution
deploy:                                                         # deployment pipeline
  stage:                                                        # deployment pipeline stage
    infra:                                                      # infrastructure pipeline stage is expecting the a KCC endpoint
      requires:                                                 # describesa list of required components for the solution to function
        useConfigConnectorSA: "true|false"                      # Pull the SA account from the Kubernetes Config Controller, use the kubeContext settings if they have been set.
        iam:                                                    # setup the following IAM roles to apply to the implementor of the solution
          - role:  "roles/gcp.reader"                                 # role to be applied
            member: "user|serviceAccount|group:acct@example.com"      # account in which to apply the role
            resource:                                                 # add role binding to a resource
              level: "organization|project"                           # bindings are added at either the organization or project level
              id: "123abc"                                            # organization ID or Project ID
      kubeContext:                                              # Kube Context configuration
        clusterName: "cluster name"                             # the name of the GKE cluster in which KCC is installed and setup on
        region: "gcp region"                                    # the region the GKE cluster is in. Set if the cluster is regional else use zone
        project: "my-kcc-project"                               # the GCP project ID
        zone: "gcp zone"                                        # the zone the GKE cluster is in. Only one of region or zone should be set
        internalIP: "true|false"                                # use the internal IP address of the cluster endpoint or not
    app:                                                        # application pipeline stage
      kubeContext:                                              # Kube Context configuration
        clusterName: "cluster name"                             # the name of the GKE cluster in which to intsall your application
        region: "gcp region"                                    # the region the GKE cluster is in. Set if the cluster is regional else use zone
        project: "my-kcc-project"                               # the GCP project ID
        zone: "gcp zone"                                        # the zone the GKE cluster is in. Only one of region or zone should be set
        internalIP: "true|false"                                # use the internal IP address of the cluster endpoint or not
```