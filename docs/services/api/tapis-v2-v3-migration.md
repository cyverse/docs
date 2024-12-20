# Tapis v2 (Agave) App migration to v3 for the Discovery Environment

The [Tapis v3 App docs](https://tapis.readthedocs.io/en/latest/technical/apps.html)
can be referenced when creating a new app from scratch,
and for other details beyond the scope of this migration guide,
but the easiest way to show how to migrate a Tapis v2 (Agave) app to v3
for the Discovery Environment (DE) might be with the following example.

## Tapis v2 App Definition

First consider a Tapis v2 app definition like the following:

```json
{
  "id": "CyVerse-QA-Test-App-0.1u1",
  "name": "CyVerse QA Test App",
  "defaultMaxRunTime": "00:02:00",
  "version": "0.1",
  "shortDescription": "CyVerse QA Test App",
  "longDescription": "CyVerse QA Test App for all parameter types.",
  "helpURI": "https://docs.cyverse.org/services/api_overview/",
  "tags": [
    "QA",
    "test"
  ],
  "executionType": "HPC",
  "executionSystem": "stampede2.tacc.utexas.edu",
  "deploymentPath": "/api/2/apps/qatesttool.zip",
  "deploymentSystem": "data.iplantcollaborative.org",
  "templatePath": "word-count.sh",
  "testPath": "test.sh",
  "inputs": [
    {
      "id": "requiredInput",
      "value": {
        "visible": true,
        "required": true
      },
      "details": {
        "label": "Required Input File",
        "description": "Required input file."
      },
      "semantics": {
        "minCardinality": 1,
        "maxCardinality": 1,
        "fileTypes": [
          "TEXT-0",
          "RAW-0"
        ]
      }
    },
    {
      "id": "optionalInput",
      "value": {
        "visible": true,
        "required": false
      },
      "details": {
        "label": "Optional Input File",
        "description": "Not required, excluded if empty."
      },
      "semantics": {
        "minCardinality": 0,
        "maxCardinality": 1,
        "fileTypes": [
          "TEXT-0",
          "RAW-0"
        ]
      }
    },
    {
      "id": "fixedInput",
      "value": {
        "visible": false,
        "required": true,
        "default": "agave://data.iplantcollaborative.org/home/shared/cyverse_training/example/coffee_cake.txt"
      },
      "details": {
        "description": "Fixed input."
      },
      "semantics": {
        "minCardinality": 1,
        "maxCardinality": 1,
        "fileTypes": [
          "TEXT-0",
          "RAW-0"
        ]
      }
    }
  ],
  "parameters": [
    {
      "id": "requiredOutput",
      "value": {
        "visible": true,
        "required": true,
        "type": "string",
        "default": "out.txt"
      },
      "details": {
        "label": "Output File",
        "description": "Output File command line option.",
        "argument": null
      },
      "semantics": {
        "minCardinality": 1,
        "maxCardinality": 1,
        "ontology": [
          "xs:string"
        ]
      }
    },
    {
      "id": "textInput",
      "value": {
        "visible": true,
        "required": false,
        "type": "string"
      },
      "details": {
        "label": "Text Input",
        "description": "Single-line text."
      }
    },
    {
      "id": "flagInput",
      "value": {
        "visible": true,
        "required": false,
        "type": "flag"
      },
      "details": {
        "label": "Flag Input",
        "description": "Checkbox input.",
        "argument": "-f",
        "showArgument": true
      },
      "semantics": {
        "minCardinality": 0,
        "maxCardinality": 1,
        "ontology": [
          "xs:boolean"
        ]
      }
    },
    {
      "id": "flagInputAlt",
      "value": {
        "visible": true,
        "required": false,
        "type": "string"
      },
      "details": {
        "label": "Flag Input 2",
        "description": "Another Checkbox input.",
        "argument": "-f2",
        "showArgument": true
      },
      "semantics": {
        "minCardinality": 0,
        "maxCardinality": 1,
        "ontology": [
          "xs:boolean"
        ]
      }
    },
    {
      "id": "integerInput",
      "value": {
        "visible": true,
        "required": false,
        "type": "number"
      },
      "details": {
        "label": "Integer",
        "description": "Integer input."
      },
      "semantics": {
        "minCardinality": 0,
        "maxCardinality": 1,
        "ontology": [
          "xs:int"
        ]
      }
    },
    {
      "id": "doubleInput",
      "value": {
        "visible": true,
        "required": false,
        "type": "number"
      },
      "details": {
        "label": "Decimal",
        "description": "Decimal input."
      }
    },
    {
      "id": "listInput",
      "value": {
        "visible": true,
        "required": false,
        "type": "enumeration",
        "order": 0,
        "enquote": false,
        "default": null,
        "enum_values": [
          {
            "val1": "Value 1"
          },
          {
            "val2": "Value 2"
          },
          {
            "val3": "Value 3"
          }
        ]
      },
      "details": {
        "label": "Selection List",
        "description": "List input.",
        "argument": "--list ",
        "showArgument": true
      },
      "semantics": {
        "minCardinality": 0,
        "maxCardinality": 1,
        "ontology": []
      }
    }
  ],
  "outputs": [
    {
      "id": "requiredOutput",
      "value": {
        "default": "out.txt"
      },
      "details": {
        "label": "Output File",
        "description": "Output File command line option."
      },
      "semantics": {
        "minCardinality": 1,
        "maxCardinality": 1,
        "ontology": [],
        "fileTypes": [
          "TEXT-0"
        ]
      }
    }
  ]
}
```

## Tapis v3 App Definition

That v2 app could be migrated to v3 like the following:

```json
{
  "id": "CyVerse-QA-Test-App",
  "version": "0.1",
  "tenant": "cyverse",
  "description": "QA Test app for all parameter types.",
  "runtime": "DOCKER",
  "containerImage": "discoenv/qatesttool",
  "jobType": "FORK",
  "tags": [
    "QA",
    "test"
  ],
  "notes": {
    "name": "CyVerse QA Test App",
    "helpURI": "https://docs.cyverse.org/services/api_overview/",
    "outputs": [
      {
        "name": "requiredOutput",
        "description": "Output File command line option.",
        "arg": "out.txt",
        "details": {
          "label": "Output File"
        }
      }
    ]
  },
  "jobAttributes": {
    "execSystemId": "cyverse-qacondor1-qa-test3",
    "maxMinutes": 2,
    "parameterSet": {
      "containerArgs": [
        {
          "name": "workdir",
          "arg": "-w /TapisOutput",
          "inputMode": "FIXED"
        }
      ],
      "envVariables": [
        {
          "key": "NEW_ENV_VAR",
          "value": "testing env",
          "inputMode": "FIXED"
        }
      ],
      "appArgs": [
        {
          "name": "requiredOutput",
          "description": "Output File command line option.",
          "arg": "out.txt",
          "inputMode": "REQUIRED",
          "notes": {
            "details": {
              "label": "Output File"
            },
            "value": {
              "default": "out.txt"
            }
          }
        },
        {
          "name": "textInput",
          "description": "Single-line text.",
          "notes": {
            "details": {
              "label": "Text Input"
            }
          }
        },
        {
          "name": "flagInput",
          "description": "Checkbox input.",
          "arg": "-f",
          "notes": {
            "details": {
              "label": "Flag Input"
            },
            "value": {
              "type": "flag"
            }
          }
        },
        {
          "name": "flagInputAlt",
          "description": "Another Checkbox input.",
          "arg": "-f2",
          "notes": {
            "details": {
              "label": "Flag Input 2"
            },
            "value": {
              "type": "string"
            },
            "semantics": {
              "ontology": [
                "xs:boolean"
              ]
            }
          }
        },
        {
          "name": "integerInput",
          "description": "Integer input.",
          "notes": {
            "details": {
              "label": "Integer"
            },
            "value": {
              "type": "number"
            },
            "semantics": {
              "ontology": [
                "xs:int"
              ]
            }
          }
        },
        {
          "name": "doubleInput",
          "description": "Decimal input.",
          "notes": {
            "details": {
              "label": "Decimal"
            },
            "value": {
              "type": "number"
            }
          }
        },
        {
          "name": "listInput",
          "description": "List input.",
          "notes": {
            "value": {
              "visible": true,
              "required": false,
              "type": "enumeration",
              "order": 0,
              "default": null,
              "enum_values": [
                {
                  "--list val1": "Value 1"
                },
                {
                  "--list val2": "Value 2"
                },
                {
                  "--list val3": "Value 3"
                }
              ]
            },
            "details": {
              "label": "Selection List"
            }
          }
        },
        {
          "name": "requiredInput",
          "description": "Required input file.",
          "inputMode": "REQUIRED",
          "notes": {
            "details": {
              "label": "Required Input File"
            }
          }
        },
        {
          "name": "optionalInput",
          "description": "Not required, excluded if empty.",
          "notes": {
            "details": {
              "label": "Optional Input File"
            }
          }
        }
      ]
    },
    "fileInputs": [
      {
        "name": "requiredInput",
        "description": "Required input.",
        "inputMode": "REQUIRED",
        "targetPath": "*"
      },
      {
        "name": "optionalInput",
        "description": "Not required, excluded if empty.",
        "targetPath": "*"
      },
      {
        "name": "fixedInput",
        "description": "Fixed input.",
        "inputMode": "FIXED",
        "sourceUrl": "tapis://data.cyverse.rocks/home/shared/cyverse_training/example/coffee_cake.txt",
        "targetPath": "*"
      }
    ]
  }
}
```

## Tapis v3 Migration Details

### The `notes` Field

Some parameter fields are no longer used in Tapis v3,
but they can be moved into a `notes` field for use by the DE,
such as app and parameter `name` and `label` display fields.

The DE will use the app's `notes.label` or `notes.name`,
followed by the app's `version`,
when displaying the app name in listings or the in the app launch form.
If no `label` or `name` is found in the app's `notes`,
then the DE will simply use the app's `id`.

### Parameters and App Args

Now take the following v2 parameter, for example:

```json
{
  "id": "textInput",
  "value": {
    "visible": true,
    "required": false,
    "type": "string",
    "default": "default_value"
  },
  "details": {
    "label": "Text Input",
    "description": "Single-line text."
  }
}
```

This parameter can be defined in v3 `appArgs` like the following:

```json
{
  "name": "textInput",
  "description": "Single-line text.",
  "arg": "default_value",
  "notes": {
    "details": {
      "label": "Text Input"
    }
  }
}
```

The `id` field becomes `name`, the `description` moves to the top level,
and the `value.default` field becomes the `arg` field;
otherwise the remaining fields move into the `notes` field.

If a `notes.details.label` field is not present,
then the DE will use the `name` as the parameter's form field label.

Note that the `visible`, `required`, and `type` fields are omitted in this example,
since those are the defaults the DE will use for those parameter fields.

If the v3 parameter has an `inputMode` field with a `REQUIRED` value,
then the DE will treat that parameter as required and ignore the
`notes.value.required` field.

If the v3 parameter has an `inputMode` field with a `FIXED` value,
then the DE will treat that parameter as hidden and ignore the
`notes.value.visible` field.

The `notes` object can contain any other custom fields
(so the entire v2 parameter can be copied as the v3 `notes` field),
which will be ignored by the DE,
except for certain fields in specific parameter types,
detailed in the following sections.

### Parameter Types

#### Inputs

If an input value is also required as a parameter on the command line,
then the `name` of the input in the `fileInputs` parameter field
should match the `name` of the corresponding `appArgs` parameter.

If the app's `runtime` value is `DOCKER`,
then the DE will automatically prepend `/TapisInput/`
to the file or folder name submitted by the user,
since that is the directory mounted inside the container by Tapis for inputs.
Otherwise only the base name of the file or folder will be submitted
for the command line argument.

#### Outputs

The `outputs` field is not used by Tapis, but if a user wishes to create a
pipeline workflow with multiple Tapis and DE apps,
then the DE requires an app output to be defined so that it can be used as an
app's input in a subsequent step in the workflow.
If the output value is also required as a parameter on the command line,
then the `name` of the output in the `notes` field should match the `name` of the
corresponding `appArgs` parameter.

If the app's `runtime` value is `DOCKER`,
then the DE will automatically prepend `/TapisOutput/`
to the parameter value submitted by the user,
since that is the directory mounted inside the container by Tapis for outputs.
Otherwise only the base name of the file or folder will be submitted
for the command line argument.

#### Flags or Booleans

If the parameter has a `notes.value.type` field value of `bool`, `boolean`, or `flag`,
then the DE will display that parameter as a checkbox.
Parameters with a `notes.semantics.ontology` list where the first value is
`xs:boolean` will also display as a checkbox.

Note that the value of a v3 parameter's `arg` is used in job submissions
when the user checks a flag parameter type in the submission form,
and the `notes.details.argument` field is not used.

#### Enumeration Lists

If the parameter has a `notes.value.type` field value of `enumeration`,
then the DE will display that parameter as a selection list.

The parameter should also have a `notes.value.enum_values` field
with an array of object values.

Each of these enum objects should have a key
that is the full command line argument that should be submitted as the parameter's `arg` value in the job submission,
and a value that will be used as the display label by the DE in the form's selection list.

As with Flag/Boolean parameter types, the `notes.details.argument` field is not used.

#### Numbers

If the parameter has a `notes.value.type` field value of `number`,
then the DE will display that parameter as a number field.
The DE will use the first `xs:*` value in the `notes.semantics.ontology` field to
determine if the user's input should be restricted to integers or decimal values.

See https://github.com/cyverse-de/mescal/blob/main/src/mescal/agave_de_v2/params.clj
for a list of supported number XSD types (otherwise defaulting to decimal values).

## Migration helper `jq`

The following [`jq`](https://jqlang.github.io/jq/) command can be used to bootstrap a migration from a v2 app into a v3 format.
It's not exhaustive, so some manual editing of the output will be required,
particularly for the `execSystemId`, `containerImage`, and `enumeration` type parameter fields;
but it can at least help with some tedious tasks,
such as copying v2 fields used by the DE into v3 `notes` fields.

```
jq 'def params: . | {"name": .id, "arg": ((.details.argument // .value.default) | tostring), "description": .details.description, "inputMode": (.value.required | if . then "REQUIRED" else "INCLUDE_ON_DEMAND" end), "notes": .} | if (.description | length) > 0 then . else del(.description) end; {id, version, "description": .longDescription, "runtime": "ZIP", "containerImage": "tapis://\(.deploymentSystem)\(.deploymentPath)", "jobType": "BATCH", tags, "jobAttributes":{"execSystemId": "cyverse-qacondor1-qa-test3", "parameterSet": {"appArgs": [.parameters[] | params]}, "fileInputs": [.inputs[] | params | {"targetPath": "*"} + .]}, "notes": {name, "label": .label, owner, shortDescription, longDescription, helpURI, ontology, executionType, executionSystem, deploymentPath, deploymentSystem, templatePath, testPath, modules, outputs}}' v2_app.json
```

Note that Tapis no longer automatically passes job parameters to the template wrapper script as environment variables by the parameter ID,
so the app's wrapper script will also need to be updated to use command line arguments
instead of env vars.

If the wrapper script was something like the following:
```
singularity exec $IMG run_prodigal -o "prodigal-out" ${QUERY} ${WRITE_PROT} ${CLOSED_ENDS} ${WRITE_NUCL} ${OUTPUT_FORMAT} ${NS_AS_MASKED} ${BYPASS_SHINE_DALGARNO} ${PROCEDURE} ${WRITE_GENES}
```

Then it could be simplified for v3 without relying on environment variables:
```
singularity exec $IMG run_prodigal -o "prodigal-out" "$@"
echo $? > "${_tapisSysRootDir}${_tapisExecSystemOutputDir}/tapisjob.exitcode"
```
See https://tapis.readthedocs.io/en/latest/technical/jobs.html#zip about `tapisjob.exitcode`.


If the app's wrapper script has logic that depends on the environment variable values,
and it would be too difficult to convert the wrapper script for command line arguments,
then use the following `jq` command, which will populate the v3 app's `envVariables` array
instead of the `appArgs` parameter array:

```
jq 'def params: . | {"name": .id, "arg": ((.details.argument // .value.default) | tostring), "description": .details.description, "inputMode": (.value.required | if . then "REQUIRED" else "INCLUDE_ON_DEMAND" end), "notes": .} | if (.description | length) > 0 then . else del(.description) end; {id, version, "description": .longDescription, "runtime": "ZIP", "containerImage": "tapis://\(.deploymentSystem)\(.deploymentPath)", "jobType": "BATCH", tags, "jobAttributes":{"execSystemId": "cyverse-qacondor1-qa-test3", "parameterSet": {"envVariables": ([.parameters[] | params] | map(.key = .name | .value = .arg | del(.name, .arg)))}, "fileInputs": [.inputs[] | params | {"targetPath": "*"} + .]}, "notes": {name, "label": .label, owner, shortDescription, longDescription, helpURI, ontology, executionType, executionSystem, deploymentPath, deploymentSystem, templatePath, testPath, modules, outputs}}' v2_app.json
```

The DE will process and display `envVariables` fields the same way that
`appArgs` parameter fields are processed and displayed,
as described in the previous sections,
except the env var's `key` and `value` are used in place of `name` and `arg`.

With the help of this guide and the
[Tapis v3 docs](https://tapis.readthedocs.io/en/latest/technical/apps.html),
hopefully the migration will not be too difficult.

# Tapis v3 `curl` Commands

The following shell environment variables, aliases, and curl commands
can be used to publish v3 app JSON (such as the JSON output above).

## Set TAPIS_ACCESS_TOKEN and TAPIS_AUTH_HEADER env vars

Assuming the env vars `CYVERSE_USER` and `CYVERSE_PASSWORD` are set:

    export TAPIS_ACCESS_TOKEN=$(curl -H "Content-Type: application/json" -s -d "{\"username\": \"$CYVERSE_USER\", \"password\": \"$CYVERSE_PASSWORD\", \"grant_type\": \"password\" }" https://cyverse.tapis.io/v3/oauth2/tokens | jq -r '.result.access_token')

---

    export TAPIS_AUTH_HEADER="X-Tapis-Token: $(echo $TAPIS_ACCESS_TOKEN | jq -r .access_token)"

## Set a curl alias that references the $TAPIS_AUTH_HEADER

    alias curl-tapis='curl -H "Content-Type: application/json" -H "$TAPIS_AUTH_HEADER"'

## List Systems

    curl-tapis -s "https://cyverse.tapis.io/v3/systems?listType=ALL"

## Apps

### Create an App

    curl-tapis -s -d @app-wc.json https://cyverse.tapis.io/v3/apps

### Get App Details

    curl-tapis -s "https://cyverse.tapis.io/v3/apps/cyverse-word-count-psarando?select=id,jobAttributes.execSystemId,version,updated,tenant,description,isPublic,owner,enabled"

### Update an App

    curl-tapis -s -X PUT -d @app-wc.json "https://cyverse.tapis.io/v3/apps/cyverse-word-count-psarando/0.1"

---

    curl-tapis -s -X PATCH -d '{"jobAttributes": {"execSystemId": "cyverse-qacondor1-qa-test3"}}' "https://cyverse.tapis.io/v3/apps/cyverse-hello-world-psarando/0.1"

---

    curl-tapis -s -X PATCH -d '{"jobAttributes": {"archiveSystemId": "cyverse-irods-qa"}}' "https://cyverse.tapis.io/v3/apps/cyverse-hello-world-psarando/0.1"

---

    curl-tapis -s -X PATCH -d '{"tags": ["test-tag", "testing"]}' "https://cyverse.tapis.io/v3/apps/cyverse-hello-world-psarando/0.1"

### Make App Public

    curl-tapis -s -X POST "https://cyverse.tapis.io/v3/apps/share_public/cyverse-hello-world-psarando"

---

    curl-tapis -s -X POST "https://cyverse.tapis.io/v3/apps/unshare_public/cyverse-hello-world-psarando"

## Jobs

### List Jobs

    curl-tapis -s "https://cyverse.tapis.io/v3/jobs/list?limit=2&orderBy=lastUpdated(desc),name(asc)&computeTotal=true" | jq .

### Submit a Job

    curl-tapis -s -d @job.json https://cyverse.tapis.io/v3/jobs/submit | jq .

### Get Job Status

    curl-tapis -s "https://cyverse.tapis.io/v3/jobs/5f660fd9-0718-4935-92ad-a67497d45013-007/status" | jq .

### List Job History

    curl-tapis -s "https://cyverse.tapis.io/v3/jobs/5f660fd9-0718-4935-92ad-a67497d45013-007/history" | jq .

### List Job Outputs

    curl-tapis -s "https://cyverse.tapis.io/v3/jobs/5f660fd9-0718-4935-92ad-a67497d45013-007/output/list/" | jq .

## List Folder Contents

    curl-tapis -s "https://cyverse.tapis.io/v3/files/ops/cyverse-irods-qa/home/psarando/analyses/Tapis_QA_Job" | jq .
