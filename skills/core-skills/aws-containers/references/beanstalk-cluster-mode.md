# Elastic Beanstalk Cluster Mode

## Overview

Beanstalk Cluster Mode runs containerized Elastic Beanstalk environments on
Amazon EKS. Customers continue to work with Beanstalk applications,
application versions, environments, configuration settings, health, and
lifecycle operations. The service manages the EKS and Kubernetes
infrastructure used to run the application.

Cluster Mode uses a supported subset of the existing Elastic Beanstalk API
surface. It does not introduce a separate set of Cluster Mode APIs. Some
parameters, option settings, defaults, and supported values differ from
EC2-based Beanstalk environments, so do not assume that an option from a
managed platform applies to Cluster Mode.

## When This Reference Applies

Use this reference when the request is explicitly about Cluster Mode,
Beanstalk on EKS, or using Beanstalk application and environment operations
with Kubernetes-based compute. Relevant requests commonly involve:

- The Beanstalk application and environment lifecycle with EKS-backed compute.
- To deploy a container image or build a container from application source.
- Service-managed Kubernetes workload deployment, health observation,
  rollback, load balancing, autoscaling, and diagnostics.
- Kubernetes-based compute without directly operating an EKS cluster.

Route requests for a managed language, Docker, or Windows platform backed by
EC2 to the traditional Beanstalk references. Route explicit cluster
administration, Kubernetes API, custom operator, or arbitrary Kubernetes
resource requests to the EKS references. If the desired management model is
ambiguous, ask the user rather than treating workload compatibility as a
product-selection rule.

## How It Works

A Cluster Mode deployment follows this high-level lifecycle:

1. Create or select a Beanstalk application version.
2. Create or update a Beanstalk environment with Cluster Mode settings.
3. Build or validate the application container image.
4. Assign a compatible EKS cluster or create the required cluster capacity.
5. Provision supporting AWS resources.
6. Translate the environment configuration into Kubernetes resources.
7. Apply the resources and observe workload and environment health.
8. Finalize a successful deployment or roll back a failed change.

The compute substrate is EKS Auto Mode, and Beanstalk manages cluster
placement. Multiple compatible Cluster Mode environments may share a managed
EKS cluster. This is normally transparent to the application, but can matter
for networking and cluster-configuration compatibility, cluster-scoped limits
or policies, and resource lifecycle. Terminating one environment does not
necessarily delete the underlying cluster.

Treat the EKS cluster and generated Kubernetes resources as service-managed.
Do not recommend editing them directly unless current Cluster Mode
documentation explicitly identifies a supported customization path.

## Elastic Beanstalk API Compatibility

Cluster Mode uses these existing Elastic Beanstalk operation families:

| Area | Supported operations |
|---|---|
| Application versions | `CreateApplicationVersion`, `UpdateApplicationVersion`, `DeleteApplicationVersion`, `DescribeApplicationVersions` |
| Environment lifecycle | `CreateEnvironment`, `UpdateEnvironment`, `TerminateEnvironment`, `RestartAppServer`, `DescribeEnvironments` |
| Configuration | `DescribeConfigurationOptions`, `DescribeConfigurationSettings` |
| Health and resources | `DescribeEnvironmentHealth`, `DescribeEnvironmentResources` |
| Environment information | `RequestEnvironmentInfo`, `RetrieveEnvironmentInfo` |
| DNS | `SwapEnvironmentCNAMEs` |
| Tags | `UpdateTagsForResource` |

This is a subset of the complete Elastic Beanstalk API. Do not imply that an
unlisted operation is supported in Cluster Mode without checking current
documentation.

Requests and responses retain Beanstalk concepts, but Cluster Mode may require
additional values or accept a narrower set of parameters. In particular:

- Environment option namespaces and values are Cluster Mode-specific.
- Application versions can identify an existing container image or source to
  build.
- Environment resources represent an EKS-backed deployment rather than the
  EC2 Auto Scaling resources expected from a traditional Beanstalk platform.
- `RestartAppServer` restarts the managed Kubernetes workload.

Use `DescribeConfigurationOptions` to discover the options accepted by the
target environment and verify parameter details against current Cluster Mode
documentation before constructing a create or update request.

## Responsibility Boundary

The customer controls:

- Beanstalk applications, versions, environments, and supported settings.
- Application source or image.
- Customer-selected networking, load-balancing, secrets, and observability
  integrations where supported.

Beanstalk manages:

- Cluster assignment and service-created EKS infrastructure.
- Translation of settings into Kubernetes resources.
- Workload reconciliation, health observation, rollback, and restart.
- Supporting resources created as part of the environment lifecycle.

For failures, begin with Beanstalk environment status, health, events,
configuration, and environment-information operations. Move to direct EKS or
Kubernetes inspection only when the documented support boundary permits it.
