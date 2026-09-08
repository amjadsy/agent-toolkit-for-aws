# Amazon Elastic Beanstalk

## Overview

Domain expertise for deploying and managing workloads with Amazon Elastic
Beanstalk. Route requests according to the management interface and lifecycle
the user is asking for before applying platform-specific or configuration
guidance.

## Compute Mode Selection

| User need | Guidance |
|---|---|
| Beanstalk application lifecycle with Kubernetes-based compute | Read [beanstalk-cluster-mode.md](beanstalk-cluster-mode.md) |
| Managed language, Docker, or Windows platform on EC2 | Read [beanstalk-platforms.md](beanstalk-platforms.md) |
| Direct ownership of an EKS cluster and arbitrary Kubernetes resources | Return to [eks.md](eks.md) |

Cluster Mode is a Beanstalk deployment target, not a synonym for direct EKS.
If the user asks for Beanstalk APIs, environments, application versions, or
managed lifecycle behavior on EKS, use the Cluster Mode reference. If the user
expects to administer the cluster directly with Kubernetes or EKS APIs, use
the EKS references.

If the request establishes only that the workload is containerized, do not
choose Cluster Mode or direct EKS based on workload compatibility alone. Ask
whether the user wants the Beanstalk application-environment lifecycle or
direct Kubernetes and cluster control, or return to higher-level AWS service
selection guidance.

## When to Load Reference Files

Load the appropriate reference file based on what the user is working on:

- **Cluster Mode or Beanstalk on EKS** -> see [beanstalk-cluster-mode.md](beanstalk-cluster-mode.md)
- **EC2 platform configuration** -> see [beanstalk-configuration.md](beanstalk-configuration.md)
- **managed language, Docker, or Windows platforms** -> see [beanstalk-platforms.md](beanstalk-platforms.md)

## Resources

- [Amazon Elastic Beanstalk Documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)
- [Amazon Elastic Beanstalk API Reference](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/Welcome.html)
- [IAM Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awselasticbeanstalk.html)
