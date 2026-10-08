# ORP Node Agent

`orp-node-agent` is the standalone Go project for enrolling OpenResty instances into OpenResty Plus.

## Responsibility

- Report node identity, OpenResty version/build information, process health, host resources, and configured local status metrics to the control plane.
- Accept and report the result of explicitly authorized, fixed node operations. The initial operation is `reload` for one configured OpenResty service unit.
- Keep an outbound authenticated connection to the control plane; do not expose an unauthenticated listener on the managed node.

## Security boundary

- Agent-to-control-plane traffic will use mutual TLS and a per-node identity that can be revoked independently.
- Requests identify a registered operation and immutable task ID. They cannot provide a shell command, executable path, service unit, or arbitrary arguments.
- Local configuration fixes the OpenResty binary, configuration path, and service unit. Reload permission is limited to that fixed target.
- The node's Control API remains an independent capability. The Agent does not call it; the Agent's reload operation uses its own restricted local service action.
- State reports and task results are attributable to the enrolled node and retained by the control plane for audit.

## Planned layout

```text
cmd/orp-node-agent/    Agent process entry point
internal/config/             Validated local configuration
internal/collector/          OpenResty and host state collection
internal/controlplane/       Authenticated heartbeat and task transport
internal/operations/         Fixed, allowlisted local operations
deploy/systemd/              Restricted service and reload helper examples
```

## Project status

This directory establishes the project boundary and security contract. The Agent executable, enrollment API, heartbeat ingestion, task queue, installer, and production OpenResty integration are not implemented yet. The existing control plane still only manages local Compose OpenResty nodes for publish operations; creating this project directory does not enable external-node publishing.
