---
editable: false
sourcePath: en/_cli-ref/cli-ref/managed-services/organization-manager/oslogin/profile/index.md
---

# yc organization-manager oslogin profile

Manage OS Login profiles

#### Command Usage

Syntax: 

`yc organization-manager oslogin profile <command>`

Aliases: 

- `profiles`

#### Command Tree

- [yc organization-manager oslogin profile get](get.md) — Show information about the specified OS Login profile
- [yc organization-manager oslogin profile create](create.md) — Create an OS Login profile
- [yc organization-manager oslogin profile update](update.md) — Update the specified OS Login profile
- [yc organization-manager oslogin profile list](list.md) — List OS Login profiles
- [yc organization-manager oslogin profile delete](delete.md) — Delete the specified OS Login profile
- [yc organization-manager oslogin profile set-default](set-default.md) — Set default OS Login profile

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
