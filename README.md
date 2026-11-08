# New Relic MCP server

Inspect accessible accounts then add approved entity tags. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `NEW_RELIC_USER_KEY`
- `NEW_RELIC_HOST`



Credential help: https://docs.newrelic.com/docs/apis/nerdgraph/get-started/nerdgraph-explorer/

## Supported scope

- Inspect data.taggingAddTagsToEntity.errors in the returned provider body. Nested mutation errors are returned without automatic success classification.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call get_current_user '{}'
```

## Use with an MCP client

Launch `node /absolute/path/new-relic-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| get_current_user | Read | Get current user. |
| list_accounts | Read | List accounts. |
| add_entity_tags | Explicit approval required | Add entity tags. |

## Concrete workflow

Inspect accessible accounts then add approved entity tags. Read provider data first, then explicitly approve the selected change with operator write opt-in and a unique per-call approval key.

1. get_current_user
2. list_accounts
3. add_entity_tags

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=graphql-to-mcp&utm_source=github&utm_campaign=github-new-relic-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/new-relic/).

## Provenance and maintenance

- https://docs.newrelic.com/docs/apis/nerdgraph/get-started/nerdgraph-explorer/
- https://api.newrelic.com/graphiql
- https://docs.newrelic.com/docs/apis/nerdgraph/examples/nerdgraph-tagging-api-tutorial/
- https://raw.githubusercontent.com/newrelic/newrelic-client-go/main/pkg/entities/tags.go

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
