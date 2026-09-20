---
type: API Endpoint
title: "OAI-ORE"
description: "Endpoint for generating OAI-ORE descriptions of a dataset."
tags: [api, terrain, endpoints, filesystem]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
# Generating OAI-ORE files for a data set.

Secured Endpoint: POST /secured/filesystem/{data-id}/ore/save

Delegates to data-info: POST /data/{data-id}/ore/save

This endpoint is a passthrough to the data-info endpoint above.
Please see the data-info documentation for more information.
