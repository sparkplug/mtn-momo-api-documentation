---
title: Introduction
---

# Introduction

The purpose of this site is to detail the design principles, objects, behaviours and error handling for the MTN Mobile Money API. The overriding goal of the API is to enable all parties to implement MTN Mobile Money APIs in a flexible, yet consistent manner. We hope to  achieve this by the implementing the following principles:

 - Use of REST architectural principles
 - Providing a set of well-defined objects that are abstracted from the underlying object representations held in the various mobile money systems. This allows an API client to construct an API message without requiring specific knowledge of the target server implementation.
 - Creation of a standard set of transaction types and other key enumerations, removing the need for developers to map for each and every API implementation.
 - Use of ISO international standards for enumerators such as currency and country codes
 - Use of supplementary metadata and sub-types to enable use case and/or mobile money provider-specific properties to be conveyed where necessary.
 - Recognising that no common mobile money account identifier exists, use of a flexible construct to enable the target account(s) and transaction parties to be identified using one or multiple identifier types.

The Open API is as JSON REST API that is used by Partner systems to access services in the Wallet platform. The Open API exposes services that are used by e.g. online merchants for managing payments and other financial services. This document gives an overview of the structure of the API.


 <!-- ![MTN MoMo Logo](./MoMo.png) -->
