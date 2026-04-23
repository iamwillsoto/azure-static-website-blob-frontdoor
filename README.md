# Azure Static Website Delivery on Azure Blob Storage with Front Door

## Overview

This project delivers a public-facing static website on Azure without relying on server-based infrastructure. Azure Blob Storage static website hosting serves as the origin, Azure Front Door provides the public edge layer, and SAS-based validation demonstrates controlled access to private storage objects.

The result is a lean, cloud-native delivery pattern aligned to the actual needs of a static web workload: low operational overhead, secure public access, and clean separation between public content delivery and restricted object access.

---

## Business Context

A static marketing site does not justify the cost or operational burden of virtual machines, web server administration, patching, or persistent compute. Hosting it on traditional infrastructure creates unnecessary complexity for a workload that only needs durable storage and reliable content delivery.

This implementation replaces that model with managed Azure services. Blob Storage hosts the site, Front Door handles secure public delivery and edge caching, and SAS is used to validate temporary access to private blob content without exposing it broadly.

---

## What This Solves

- Eliminates the need to run a static website on server infrastructure
- Reduces operational overhead by using managed Azure services
- Enforces HTTPS at the edge through Azure Front Door
- Improves delivery behavior with edge caching
- Demonstrates a controlled access pattern for private storage objects using SAS

---

## Implementation Summary

The site was deployed to an Azure Storage Account with static website hosting enabled and `index.html` configured as the default document in the `$web` container. Azure Front Door Standard was placed in front of the storage-hosted site and configured to use the static website endpoint as the origin. Public traffic was routed through Front Door with HTTP redirected to HTTPS, and cache behavior was validated by updating site content and purging edge cache.

A separate private container was then used to validate restricted blob access. Direct anonymous access to the private object failed as expected, while a SAS-based URL provided temporary, scoped access to the same content.

---

## Services Used

- Azure Resource Group
- Azure Storage Account
- Azure Blob Storage static website hosting
- Azure Front Door Standard
- Shared Access Signatures (SAS)

---

## Architecture Summary

- Azure Storage Account hosts the static site through the `$web` container
- Azure Front Door provides the public endpoint and edge routing layer
- HTTPS redirection is enforced at Front Door
- Cache refresh behavior is validated through update and purge testing
- A private blob container is used to validate restricted access with SAS

---

## Validation Summary

- The Azure static website endpoint served the Crestline Bank site successfully
- Azure Front Door served the site publicly with HTTPS redirection in place
- Content updates were reflected at the origin and validated through Front Door after cache purge
- Direct access to a private blob failed without authorization
- Temporary access to the private blob succeeded through SAS

---

## Key Notes

- The public site is served through Azure static website hosting in the `$web` container
- Front Door is configured against the static website origin, not the standard blob service endpoint
- Cache propagation delay was treated as expected CDN behavior and validated through purge testing
- SAS validation was performed against a private container to demonstrate controlled access without making the object public
- Any SAS token referenced in documentation should be redacted before publication

---

## Outcome

This project demonstrates a clean Azure pattern for static website delivery using managed services instead of server infrastructure. It reduces operational burden, improves public delivery posture, and validates a separate access model for private storage content. The design is simple, defensible, and aligned to the requirements of a static web workload.