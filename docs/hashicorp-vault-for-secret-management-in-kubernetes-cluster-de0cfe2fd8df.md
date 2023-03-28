# Kubernetes 集群中用于秘密管理的 Hashicorp 保险库

> 原文：<https://medium.com/geekculture/hashicorp-vault-for-secret-management-in-kubernetes-cluster-de0cfe2fd8df?source=collection_archive---------11----------------------->

## 介绍

Hashicorp Vault 提供了 Vault 的所有功能和安全性，没有自行管理的复杂性和开销。它还提供各种身份验证方法，如 AWS、Kubernetes、令牌、OIDC、Azure Active Directory 等。在 EC2 机器、Kubernetes pods 等基础设施中提供和动态注入秘密。
HashiCorp 云平台采用网络用户界面来部署和管理资源，包括 AWS 中的 HCP 金库部署。然而，如果你喜欢自动 HCP 保险库…