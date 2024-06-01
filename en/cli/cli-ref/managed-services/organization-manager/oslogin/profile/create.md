---
editable: false
sourcePath: en/_cli-ref/cli-ref/managed-services/organization-manager/oslogin/profile/create.md
---

# yc organization-manager oslogin profile create

Create an OS Login profile

#### Command Usage

Syntax: 

`yc organization-manager oslogin profile create [Flags...] [Global Flags...]`

#### Flags

| Flag | Description |
|----|----|
|`--organization-id`|<b>`string`</b><br/>Set the ID of the organization to use.|
|`--async`|Display information about the operation in progress, without waiting for the operation to complete.|
|`--login`|<b>`string`</b><br/>A login for a posix OS Login profile.|
|`--uid`|<b>`int`</b><br/>A uid for a posix OS Login profile.|
|`--home-directory`|<b>`string`</b><br/>A home directory for a posix OS Login profile.|
|`--shell`|<b>`string`</b><br/>A shell for a posix OS Login profile.|
|`--subject-id`|<b>`string`</b><br/>The ID of the subject whom profile belongs to.|

#### Global Flags

| Flag | Description |
|----|----|
|`--profile`|<b>`string`</b><br/>Set the custom configuration file.|
|`--debug`|Debug logging.|
|`--debug-grpc`|Debug gRPC logging. Very verbose, used for debugging connection problems.|
|`--no-user-output`|Disable printing user intended output to stderr.|
|`--retry`|<b>`int`</b><br/>Enable gRPC retries. By default, retries are enabled with maximum 5 attempts.<br/>Pass 0 to disable retries. Pass any negative value for infinite retries.<br/>Even infinite retries are capped with 2 minutes timeout.|
|`--cloud-id`|<b>`string`</b><br/>Set the ID of the cloud to use.|
|`--folder-id`|<b>`string`</b><br/>Set the ID of the folder to use.|
|`--folder-name`|<b>`string`</b><br/>Set the name of the folder to use (will be resolved to id).|
|`--endpoint`|<b>`string`</b><br/>Set the Cloud API endpoint (host:port).|
|`--token`|<b>`string`</b><br/>Set the OAuth token to use.|
|`--impersonate-service-account-id`|<b>`string`</b><br/>Set the ID of the service account to impersonate.|
|`--no-browser`|Disable opening browser for authentication.|
|`--format`|<b>`string`</b><br/>Set the output format: text (default), yaml, json, json-rest.|
|`-h`,`--help`|Display help for the command.|
