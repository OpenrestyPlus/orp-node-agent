# ORP Node Agent

Project identifier: `orp-node-agent`.

The Agent is an independent Go project for enrolling external OpenResty instances into OpenResty Plus.

## Planned responsibilities

- Report node identity, OpenResty version/build information, process health, host resources, and configured local status metrics.
- Maintain an outbound authenticated connection to the control plane using mutual TLS and a revocable per-node identity.
- Execute only explicitly authorized fixed operations. The initial operation is reload of one locally configured OpenResty service unit; requests cannot supply shell commands, executable paths, units, or arbitrary arguments.
- Keep the node's Control API independent. The Agent does not call that API.

## Planned layout

```text
cmd/orp-node-agent/    Agent process entry point
internal/config/             Validated local configuration
internal/collector/          OpenResty and host state collection
internal/controlplane/       Authenticated heartbeat and task transport
internal/operations/         Fixed, allowlisted local operations
deploy/systemd/              Restricted service and reload helper examples
```

## Status

This repository currently contains the project charter only. The executable, enrollment API, heartbeat ingestion, task queue, installer, and production node integration are not implemented. The control plane still supports publishing only to local Compose nodes.
