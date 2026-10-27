# Azure Developer Tools

Install Azure-focused GitHub Copilot plugins that help you build Hosted Skills,
inspect Azure resources, diagnose apps with SRE Agent, review cost health,
browse Azure Storage, operate local containers, and create canvas apps.

## Install a plugin

Add the production marketplace once:

```sh
copilot plugin marketplace add microsoft/azure-dev-tools
```

Then install the plugin you need:

```sh
copilot plugin install azure-functions-hosted-skills@azure-dev-tools
copilot plugin install azure-resources-query@azure-dev-tools
copilot plugin install azure-cost-health-check@azure-dev-tools
copilot plugin install azure-sre-agent@azure-dev-tools
copilot plugin install azure-storage-canvas@azure-dev-tools
copilot plugin install containers@azure-dev-tools
copilot plugin install canvas-authoring@azure-dev-tools
```

You can also open GitHub Copilot **Customize > Plugins**, use the marketplace
gear to add `microsoft/azure-dev-tools` (marketplace ID `azure-dev-tools`), and
select a plugin. Fully quit and reopen GitHub Copilot after installation, then
start a fresh chat.

The marketplace follows this repository's default branch. Immutable tags are
the reproducible install points:

| Plugin | What it does | Production status |
| --- | --- | --- |
| [Azure Functions Hosted Skills](canvases/azure-functions-hosted-skills/) | Build and run local Hosted Skills or invoke supported functions in an existing Function App. | 0.5.5 is tagged; `azure-functions-hosted-skills-latest` points to that release. |
| [Azure Resources Query](canvases/azure-resources-query/) | Find and inspect Azure resources with read-only Resource Graph queries. | 0.1.6 candidate narrows inventory activation; `azure-resources-query-latest` remains on 0.1.5 until reviewed promotion. |
| [Canvas Authoring](plugins/canvas-authoring/) | Add toolkit setup and starter apps to the native `create-canvas` workflow. | 0.1.1 is tagged; `canvas-authoring-latest` points to that release. Skill-only plugin, with no canvas panel. |
| [Azure Cost Health Check](canvases/azure-cost-health-check/) | Review spend, forecasts, budgets, alerts, recommendations, and AI billing. | 0.4.5 is tagged; `azure-cost-health-check-latest` points to that release. |
| [Azure SRE Agent](canvases/azure-sre-agent/) | Connect to your SRE Agent, diagnose failing apps, and investigate incident threads. | 0.2.7 is tagged; `azure-sre-agent-latest` points to that release. |
| [Azure Storage canvas](canvases/azure-storage-canvas/) | Browse blobs, inspect statistics, download files, and plan storage transfers with Copilot. | 0.1.1; immutable and `azure-storage-canvas-latest` install links become usable after the reviewed merge and tag verification. |
| [Containers](canvases/containers/) | Browse and operate local Docker or Podman containers and images. | 0.1.1 candidate; immutable and `containers-latest` install links become usable after the reviewed merge and tag verification. |

Native App marketplace installation has not been verified for every product;
a source package or direct CLI install alone does not establish that the
native listing is live.

## Start with a useful prompt

**Hosted Skills**

> Open Azure Functions Hosted Skills canvas so I can try a daily GitHub
> repository digest for octocat/Hello-World with the Timer trigger.

**Resources Query**

> Show my Function Apps in Development.

**Cost Health Check**

> Open Azure Cost Health Check in real mode for my subscription.

**Azure SRE Agent**

> My app is failing. Open SRE Agent Canvas so I can investigate it.

**Canvas Authoring**

> Use the native create-canvas skill and the create-canvas-app companion to
> build a counter canvas.

**Azure Storage**

> Open Azure Storage canvas.

**Containers**

> Open Containers canvas.

Each product README lists its prerequisites, safe prompts, first-run workflow,
version-pinned installation options, and product-specific safety behavior.

## Disable usage telemetry

For plugins whose **Usage data** notice links here, this preference disables
their canvas and skill usage telemetry without disabling plugin features.
Older versions without this preference must be updated first.

1. Create a `canvases` folder under `~/.copilot`, then create
   `telemetry.json` inside it. On Windows, the default path is
   `%USERPROFILE%\.copilot\canvases\telemetry.json`. If the provider has
   `COPILOT_HOME` configured, use `<COPILOT_HOME>/canvases/telemetry.json`
   instead.
2. Save this JSON in the file:

   ```json
   {
     "enabled": false
   }
   ```

3. Fully quit and reopen the app running the plugin, then start a fresh
   session. For Copilot plugins, restart GitHub Copilot; for a Claude Desktop
   MCP App whose notice links here, restart Claude Desktop. Existing
   collectors do not reload this file while running.

Create the file for the user and on the machine running the extension or MCP
server. A file on your laptop does not configure a remote provider. This is
a plugin preference, not a built-in Copilot or enterprise-managed setting.
It does not control the host app's telemetry, Azure Application Insights,
or other plugins that do not support this preference.

To restore the plugin's normal collection policy, remove the file or set
`"enabled": true`, then restart again. The file must contain valid JSON with
a boolean `enabled`, be a regular file, and be no larger than 4,096 bytes.
An invalid or unreadable preference disables collection and reports a local
diagnostic; normal plugin features remain available.

## Get involved

See [CONTRIBUTING.md](CONTRIBUTING.md) to contribute. For help or feedback, see
[SUPPORT.md](SUPPORT.md). To report a security vulnerability, follow
[SECURITY.md](SECURITY.md). This repository is licensed under the
[MIT License](LICENSE).
