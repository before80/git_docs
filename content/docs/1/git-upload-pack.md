https://git-scm.com/docs/git-upload-pack

## 名称

git-upload-pack - Send objects packed back to git-fetch-pack

## 概述

```
git-upload-pack [--[no-]strict] [--timeout=<n>] [--stateless-rpc]
		  [--advertise-refs] <directory>
```

## 描述

Invoked by *git fetch-pack*, learns what objects the other side is missing, and sends them after packing.

This command is usually not invoked directly by the end user. The UI for the protocol is on the *git fetch-pack* side, and the program pair is meant to be used to pull updates from a remote repository. For push operations, see *git send-pack*.

## 选项

- --[no-]strict

  Do not try <directory>/.git/ if <directory> is no Git directory.

- --timeout=<n>

  Interrupt transfer after <n> seconds of inactivity.

- --stateless-rpc

  Perform only a single read-write cycle with stdin and stdout. This fits with the HTTP POST request processing model where a program may read the request, write a response, and must exit.

- --http-backend-info-refs

  Used by [git-http-backend[1]](../git-http-backend) to serve up `$GIT_URL/info/refs?service=git-upload-pack` requests. See "Smart Clients" in [gitprotocol-http[5]](../../5/gitprotocol-http) and "HTTP Transport" in the [gitprotocol-v2[5]](../../5/gitprotocol-v2) documentation. Also understood by [git-receive-pack[1]](../git-receive-pack).

- <directory>

  The repository to sync from.

## ENVIRONMENT

- `GIT_PROTOCOL`

  Internal variable used for handshaking the wire protocol. Server admins may need to configure some transports to allow this variable to be passed. See the discussion in [git[1]](../git).

## 另请参阅

[gitnamespaces[7]](../../7/gitnamespaces)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。