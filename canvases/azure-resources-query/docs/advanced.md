# Azure Resources Query: installation and troubleshooting

Find Azure resources in GitHub Copilot, inspect their details, and add a selected set to your conversation. Azure Resources Query runs read-only Azure Resource Graph (ARG) queries in the subscriptions you choose; results stay in a browsable panel instead of filling your chat with a table.

![Azure Resources Query with sample Function Apps and Add to chat. Synthetic fixture data only.](resources-fixture.png)

*Example panel with sample resources, not your Azure data.*

The product name and Resource Graph Explorer icon stay visible as you switch
queries; the current view's title appears underneath. The panel follows your
host theme.

## Install

### Install the full plugin

In the GitHub Copilot App, open **Customize > Plugins** and install **Azure
Resources Query** from the **awesome-copilot** marketplace. This installs the
full plugin, including the canvas and launcher skill. Fully quit GitHub
Copilot, reopen it, start a new chat, and ask:

> Open the Azure Resources Query canvas.

Check **Plugin skills** for the Azure Resources Query launcher skill.

## Pinning the full plugin

The marketplace install is recommended for normal use. For a reproducible
full-plugin install, choose a published immutable tag from the
[published tags](https://github.com/microsoft/azure-dev-tools/tags) that starts
with `azure-resources-query-v` and includes its source qualifier. Confirm the
tag is published; the `azure-resources-query-latest` tag moves and is not an
exact pin. Enter the exact tag when prompted:

```sh
read -r ARQ_TAG
git clone --depth 1 --branch "$ARQ_TAG" https://github.com/microsoft/azure-dev-tools.git azure-resources-query-plugin
copilot plugin install ./azure-resources-query-plugin/canvases/azure-resources-query
```

This installs the full plugin from the selected immutable version rather than
the moving marketplace release.

## Canvas-only fallback

If the full plugin is unavailable, use **Customize > Canvases > Install from
gist/URL** and paste the nested canvas extension URL for a published immutable
tag. Choose a tag from the [published tags](https://github.com/microsoft/azure-dev-tools/tags)
that starts with `azure-resources-query-v` and includes its source qualifier,
then replace `<published-tag>` here:

```text
https://github.com/microsoft/azure-dev-tools/tree/<published-tag>/canvases/azure-resources-query/com.github.copilot/extensions/azure-resources-query
```

This installs only the canvas, not its launcher skill. Do not install this
fallback alongside the full plugin; duplicate providers can conflict. The
`azure-resources-query-latest` tag moves and is not an exact pin.

## Requirements

- A canvas-capable GitHub Copilot App with Node.js 22 or newer.
- Azure CLI 2.61 or newer on the extension provider's PATH.
- An Azure account with read access to the subscriptions and resource details
  you need. Sign in with `az login` or use **Azure connection > Sign in** in
  the canvas; opening it never signs in automatically.

## Try it

Ask Copilot:

> Show my Function Apps in Development.

Then refine the same view:

> Only those in West Europe.

Or start another resource list:

> Show storage accounts in Development.

Replace **Development** with your subscription name or ID. You can also list Web Apps, VMs, or other resources supported by ARG. Copilot writes the KQL; **Query details** shows the exact query behind the displayed results.

## First run: scope, inspect, select, add to chat

1. **Confirm scope.** Ask for a list. Without a named subscription, choose
   **Continue** to confirm an eligible CLI default, or choose subscriptions in
   the panel. Nothing is preselected or silently accepted. A unique enabled
   named match can run immediately; ambiguous names require a choice.
   Confirmation runs the supplied query once. Cancel stops it.
2. **Inspect.** Click a resource name to see its query fields and read-only ARM
   details. Use **Back to results** to return. **Copy ID** and **Open in portal**
   are available per resource.
3. **Select.** Check individual resources or **Select loaded rows**. Selection
   applies to loaded rows, not every unseen result. Use **Load more** when
   available and review any incomplete-results warning.
4. **Add to chat.** Click **Add to chat** to share the selected resource context
   with Copilot. Selection is context, not permission to change those resources.

Follow-up requests keep the current scope and refine the query. **Change**
lets you choose a different scope; **Apply** runs the current query in that
scope. **Rerun query** refreshes the displayed inventory. Sign-in/profile reload
does not rerun it automatically.

## Troubleshooting and safety

| Symptom | What to check |
| --- | --- |
| Azure CLI not found | Install Azure CLI 2.61+ and ensure `az` is on the provider's PATH, not just a terminal alias. Reload the host after correcting its environment. |
| Sign-in required or wrong account | Sign in explicitly, then **Reload CLI profile** after an external CLI change. **Refresh from Azure** is separate and can update the shared CLI profile. Neither runs KQL. |
| Permission denied | Check the account, tenant and selected subscriptions. Ask your administrator for the required read access; this canvas does not grant roles. An ARG result does not guarantee permission to read every resource's ARM details. |
| No matching subscriptions | Check the name/ID and enabled status. This is unresolved scope, not proof that there are no resources. |
| No results | Review the executed KQL, selected subscriptions and location criteria. Zero rows means no resources visible to your account matched this query; ARG can lag recent changes. |
| Panel missing or old panel reappears | Inspect the enabled provider, reload extensions once, and retry only if the canonical canvas is registered. An existing panel stays bound to its old provider; close it or use a fresh chat after correcting installation. |

Queries are read-only. Resource labels and query results are data, not
instructions. Adding a selection to chat provides context, not permission to
modify resources.
