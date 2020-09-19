# ghostblog

This is a repo to quickly build out a ghostblog CMS in kubernetes using
an ingress controller and SSL via cert-manager/lets-encrypt. The /config
directory houses manifests that can be run individually to achieve the goal of installing the ghost CMS.

The `ghostblog` directory is intended to be used with abstraction tools such as kustomize ..etc.

To create the ghostblog CMS application:
`kubectl apply -k ./ghostblog`