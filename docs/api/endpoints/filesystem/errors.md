---
type: API Endpoint
title: "Filesystem errors"
description: "Error codes returned by the filesystem endpoints."
tags: [api, terrain, endpoints, filesystem]
status: stable
generated: { by: process:okf-migration, at: 2026-07-29T00:00:00Z }
---
Error Codes
-----------

When it encounters an error, filesystem will generally return a JSON object in the form:

    {
        "error_code" : "ERR_CODE"
    }

Other entries may be included in the map, but you shouldn't depend on them being there for error checking.
