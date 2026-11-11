---
name: debug
description: Inspect a running Aspire application through its CLI or available MCP tools, diagnose blocked dependencies, and trace requests or event propagation. Use when asked to check Aspire, inspect service health or logs, or investigate local startup and synchronization.
kind: utility
domain: aspire
profile: core
applicability: Aspire
requires: Aspire CLI or Aspire MCP tools
provenance: house
---

# Debug an Aspire application

Use the Aspire CLI or available Aspire MCP tools as the normal inspection route. Use the dashboard for
an explicitly requested visual demonstration or a capability those interfaces do not provide; explain
the gap before switching to browser or desktop control.

```text
aspire ps
aspire describe --apphost <owning-AppHost.csproj> --format Json --non-interactive --nologo
aspire logs --help
# Select the affected resource and a bounded log tail using the installed command's options.
```

## Select the existing application

Resolve the owning checkout and AppHost from the task and repository instructions. Discover running
applications and select the exact AppHost. Do not start a second stack just to inspect one already
running. If none is running, use the project's documented launch path within the authorized task.

Inspect the installed CLI's help and available MCP tool schemas before choosing commands; capabilities
vary by version. CLI inspection can connect to an AppHost launched with `dotnet run`. Use whichever
programmatic interface is already usable; MCP registration is not a prerequisite for CLI inspection.
Resolve the actual installed executable on the current OS, including `aspire.cmd` when that is the
Windows installation's entry point.

If an MCP integration is needed, use the supported CLI-hosted server (`aspire agent mcp`) and the
host's native registration path. Consult the current documentation instead of assuming a dashboard
URL is an MCP endpoint. Keep the integration scoped to the intended workspace and verify AppHost
discovery after connecting. Do not introduce a network listener or expose the dashboard to obtain local
diagnostics. These interfaces still need access to the runtime environment; avoiding desktop control
does not make a workstation's local application accessible from an unrelated remote agent.

## Follow the failing dependency

Start with resource state and dependencies. For a waiting web host, inspect the failed prerequisite's
startup and console logs before exercising its routes. Distinguish image/feed authentication,
migrations and dependency readiness from defects in a running service. Use the project's selected
credential workflow for a package denial; it is not evidence of broken application event handling.

```text
Observation: web is Waiting; auth-migrations is FailedToStart.
Next evidence: auth-migrations startup/container logs and dependency state.
If the image pull is denied: resolve package access, then recheck startup.
Do not diagnose the web request handler from an endpoint that has not started.
```

Read bounded console or structured logs for the affected resources. Use a request's trace ID to follow
calls across services when telemetry is present. If telemetry is absent, state the gap and inspect
the next supported evidence source. Restart, stop and other resource commands change the application;
use them only when the task calls for that change, not as a prerequisite for inspection.

## Prove the requested behavior

Report the selected AppHost, affected resources and relevant log or trace evidence. Healthy resources
prove readiness, not that a business request succeeded. Capture the actual request status and useful
response evidence. For asynchronous synchronization, correlate the producer, consumer and resulting
persisted state; apply the application's own data ownership and seeding rules.

Use supported database/configuration inspection when the question requires database identity or
persisted state. Resource names and health alone prove neither. Keep secret values out of reports and
state unverified links in the chain instead of treating missing evidence as success. For test-suite
execution, follow the repository's test lifecycle rather than launching an additional diagnostic stack.

## Interface references

- [CLI resource inspection](https://aspire.dev/reference/cli/commands/aspire-describe/)
- [CLI logs](https://aspire.dev/reference/cli/commands/aspire-logs/)
- [Aspire MCP server](https://aspire.dev/get-started/aspire-mcp-server/)
