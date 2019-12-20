# Red Hat Marketplace - Operator Spec

## Operator Onboarding Steps

1. Remove any reference of image locations from the operator code.
   * I.e. if you have hard coded references to images in your operator for downloads.
2. Add the env vars to the with `RELATED_IMAGE_<dentifier>` to the operator CSV.
   * Example: [clusterserviceversion.yaml](https://github.com/zachncst/om-env-example/blob/master/ubi-noop-go/deploy/olm-catalog/ubi-noop-go/0.0.1/ubi-noop-go.v0.0.1.clusterserviceversion.yaml#L87-L88)
3. Reference the env variable in the operator source code.'
   * Example: [ubinoop_controller.go](https://github.com/zachncst/om-env-example/blob/master/ubi-noop-go/pkg/controller/ubinoop/ubinoop_controller.go#L138-L175)
4. Update your operator images and metadata in Red Hat's Container Certification tool.

## Description of the repo

This repo contains example operator code and metadata (for deployment using the Operator Lifecycle Manager or OLM) used to showcase the override of an operator image source using environment variables.

Refactoring your operator code to use env vars for the operand(s) image source will help ensure that your operator will work in disconnected/offline or proxied Kubernetes environments such as OpenShift Container Platform (when installed in such an environment).

## Proposed Naming Convention for Environment Variables

For compatibility with OpenShift Container Platform and the K8s platform-agnostic Operator Lifecycle Manager, the following convention is suggested when naming your environment variables:

* All env vars should start with the prefix `RELATED_IMAGE_`
* The convention follows the rough form of `RELATED_IMAGE_<identifier>`
* Any friendly name can be used for the latter part of the environment variable, however any alpha characters should be upper case (eg: `RELATED_IMAGE_DATABASE`) per bourne/bash shell conventions
* Alphanumeric or purely numeric identifiers can also be used (eg: `RELATED_IMAGE_DB0` or simply `RELATED_IMAGE_0`)
* Numerous environment variables can be defined and utilized as needed to allow any number of operand types (unique container images)
* Extended identifiers can be used by including additional underscores in the identifier (eg: `RELATED_IMAGE_ISTIO_SIDECAR`)

## Example Implementation - Quick Links

See [ubinoop_controller.go](https://github.com/zachncst/om-env-example/blob/master/ubi-noop-go/pkg/controller/ubinoop/ubinoop_controller.go#L138-L175) for an example go source implementation.

Also see [clusterserviceversion.yaml](https://github.com/zachncst/om-env-example/blob/master/ubi-noop-go/deploy/olm-catalog/ubi-noop-go/0.0.1/ubi-noop-go.v0.0.1.clusterserviceversion.yaml#L87-L88) for an example of setting these environment variables in the operator metadata.
