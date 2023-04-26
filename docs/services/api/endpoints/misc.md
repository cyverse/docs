**Jump to:**

* [Miscellaneous Terrain Endpoints](#miscellaneous-terrain-endpoints)

* [Verifying that Terrain is Running](#verifying-that-terrain-is-running)

* [Saving User Session Data](#saving-user-session-data)

* [Retrieving User Session Data](#retrieving-user-session-data)

* [Removing User Session Data](#removing-user-session-data)

* [Saving User Preferences](#saving-user-preferences)

* [Retrieving User Preferences](#retrieving-user-preferences)

* [Removing User Preferences](#removing-user-preferences)

* [Obtaining Identifiers](#obtaining-identifiers)

# Miscellaneous Terrain Endpoints

Note that secured endpoints in Terrain and apps are a little different from each other. Please see [Terrain Vs. Apps](terrain-v-apps.md) for more information.

## Verifying that Terrain is Running

Unsecured Endpoint: GET /

The root path in Terrain can be used to verify that Terrain is actually running and is responding. Currently, the response to this URL contains only a welcome message. Here's an example:

```
$ curl -s http://by-tor:8888/
The infinite is attainable with Terrain!
```

## Saving User Session Data

Secured Endpoint: POST /secured/sessions

This service can be used to save arbitrary JSON user session information. The post body is stored as-is and can be retrieved by sending an HTTP GET request to the same URL.

Here's an example:

```
$ curl -sH "$AUTH_HEADER" -d '{"foo":"bar"}' "http://by-tor:8888/secured/sessions"
```

## Retrieving User Session Data

Secured Endpoint: GET /secured/sessions

This service can be used to retrieve user session information that was previously saved by sending a POST request to the same service.

Here's an example:

```
$ curl -H "$AUTH_HEADER" "http://by-tor:8888/secured/sessions"
{"foo":"bar"}
```

## Removing User Session Data

Secured Endpoint: DELETE /secured/sessions

This service can be used to remove saved user session information. This is helpful in cases where the user's session is in an unusable state and saving the session information keeps all of the user's future sessions in an unusable state.

Here's an example:

```
$ curl -XDELETE -H "$AUTH_HEADER" "http://by-tor:8888/secured/sessions"
```

Check the HTTP status of the response to tell if it succeeded. It should return a status in the 200 range.

An attempt to remove session data that doesn't already exist will be silently ignored and return a 200 range HTTP status code.

## Saving User Preferences

Secured Endpoint: POST /secured/preferences

This service can be used to save arbitrary user preferences. The body must contain all of the preferences for the user; any key-value pairs that are missing will be removed from the preferences. Please note that the "defaultOutputDir" and the "systemDefaultOutputDir" will always be present, even if not included in the JSON passed in.

Example:

```
$ curl -sH "$AUTH_HEADER" -d '{"appsKBShortcut":"A","rememberLastPath":true,"closeKBShortcut":"Q","defaultOutputFolder":{"id":"/iplant/home/wregglej/analyses","path":"/iplant/home/wregglej/analyses"},"dataKBShortcut":"D","systemDefaultOutputDir":{"id":"/iplant/home/wregglej/analyses","path":"/iplant/home/wregglej/analyses"},"saveSession":true,"enableEmailNotification":true,"lastPathId":"/iplant/home/wregglej","notificationKBShortcut":"N","defaultFileSelectorPath":"/iplant/home/wregglej","analysisKBShortcut":"Y"}' "http://by-tor:8888/secured/preferences" | squiggles
{
    "preferences": {
        "analysisKBShortcut": "Y",
        "appsKBShortcut": "A",
        "closeKBShortcut": "Q",
        "dataKBShortcut": "D",
        "defaultFileSelectorPath": "/iplant/home/wregglej",
        "defaultOutputFolder": {
            "id": "/iplant/home/wregglej/analyses",
            "path": "/iplant/home/wregglej/analyses"
        },
        "enableEmailNotification": true,
        "lastPathId": "/iplant/home/wregglej",
        "notificationKBShortcut": "N",
        "rememberLastPath": true,
        "saveSession": true,
        "systemDefaultOutputDir": {
            "id": "/iplant/home/wregglej/analyses",
            "path": "/iplant/home/wregglej/analyses"
        }
    }
}
```

## Retrieving User Preferences

Secured Endpoint: GET /secured/preferences

This service can be used to retrieve a user's preferences.

Example:

```
$ curl -sH "$AUTH_HEADER" "http://by-tor:8888/secured/preferences" | squiggles
{
    "analysisKBShortcut": "Y",
    "appsKBShortcut": "A",
    "closeKBShortcut": "Q",
    "dataKBShortcut": "D",
    "defaultFileSelectorPath": "/iplant/home/test",
    "defaultOutputFolder": {
        "id": "/iplant/home/test/analyses",
        "path": "/iplant/home/test/analyses"
    },
    "enableEmailNotification": true,
    "lastPathId": "/iplant/home/test",
    "notificationKBShortcut": "N",
    "rememberLastPath": true,
    "saveSession": true,
    "systemDefaultOutputDir": {
        "id": "/iplant/home/test/analyses",
        "path": "/iplant/home/test/analyses"
    }
}
```

## Removing User Preferences

Secured Endpoint: DELETE /secured/preferences

This service can be used to remove a user's preferences.

Please note that the "defaultOutputDir" and the "systemDefaultOutputDir" will still be present in the preferences after a deletion.

Example:

```
$ curl -X DELETE -H "$AUTH_HEADER" "http://by-tor:8888/secured/preferences"
```

Check the HTTP status code of the response to determine success. It should be in the 200 range.

An attempt to remove preference data that doesn't already exist will be silently ignored.

## Obtaining Identifiers

Unsecured Endpoint: GET /uuid

In some cases, it's difficult for the UI client code to generate UUIDs for objects that require them. This service returns a single UUID in the response body. The UUID is returned as a plain text string.
