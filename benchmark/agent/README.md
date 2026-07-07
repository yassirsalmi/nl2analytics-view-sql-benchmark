# Analytic View Agent Specs

This directory includes two Oracle Analytic View NL2SQL agents,
`analytic_view_agent_spec.json` and `vanilla_agent_spec.json`, exported using
[Open Agent Spec](https://github.com/oracle/agent-spec). The specs are
intentionally portable: the model endpoint and database connection are
deployment responsibilities, while the exported tools define the runtime
contract.

## Files

- `analytic_view_agent_spec.json`: exported Agent Spec artifact.
- `vanilla_agent_spec.json`: baseline Agent Spec without the Analytic-View skill
  or tools.
- `analytic_view_skill.md`: the Analytic View SQL skill instructions used by the 
  analytic_view_agent_spec agent.
- `RESULTS.md`: benchmark results comparing the analytic_view_agent_spec to 
  the vanilla_agent_spec agents.

## Analytic View Skill And Tools

Oracle Analytic View SQL is not just regular table SQL with different object
names. The agent must discover measures, dimensions, hierarchies, level columns,
and AV-specific query syntax, then validate that the final SQL executes at the
right grain. The exported Agent Spec packages this as an AV SQL skill plus
specialized tools for schema overview, table metadata, analytic-view DDL,
analytic-view columns, and SQL execution.

By contrast, `vanilla_agent_spec.json` is the benchmark control. It uses only
the general `get_schema_overview`, `get_table_schemas`, and `run_sql` tools. This
makes it possible to measure the contribution of the Analytic View skill and
specialized metadata tools against a very simple baseline NL2SQL agent.

The skill itself is included as [analytic_view_skill.md](analytic_view_skill.md)
so implementers can inspect the guidance that helps the agent with writing
Oracle Analytic View SQL.

## Import The Agent

Import support depends on the Agent Spec runtime you use, but the deployment
shape should look like this:

1. Load either `analytic_view_agent_spec.json` or `vanilla_agent_spec.json` with
   your Agent Spec loader, depending on the configuration you want to run.
2. Resolve the model placeholders to your deployed model.
3. Register implementations for the tool names declared by the selected JSON.
4. Inject hidden runtime context into those tools.
5. Start a conversation or session from the loaded agent.

For example, with a Wayflow Agent Spec runtime:

```python
import json
from pathlib import Path

from wayflowcore.agentspec import AgentSpecLoader

spec_path = Path("analytic_view_agent_spec.json")  # or vanilla_agent_spec.json
spec = json.loads(spec_path.read_text())
spec["llm_config"]["model_id"] = "YOUR_MODEL_ID"
spec["llm_config"]["url"] = "YOUR_MODEL_ENDPOINT"

hidden_args = {
    "connection_factory": connection_factory,
    "schema": "YOUR_SCHEMA",
}


def bind_hidden_args(tool_fn):
    def wrapped(*args, **kwargs):
        kwargs["hidden_args"] = hidden_args
        return tool_fn(*args, **kwargs)

    return wrapped


tool_registry = {
    "get_schema_overview": bind_hidden_args(get_schema_overview),
    "get_table_schemas": bind_hidden_args(get_table_schemas),
    "get_analytic_view_ddls": bind_hidden_args(get_analytic_view_ddls),
    "get_analytic_view_columns": bind_hidden_args(get_analytic_view_columns),
    "run_sql": bind_hidden_args(run_sql),
}

# When loading the vanilla spec, only get_schema_overview, get_table_schemas,
# and run_sql are required in the registry.

agent = AgentSpecLoader(tool_registry=tool_registry).load_json(json.dumps(spec))
conversation = agent.start_conversation()
```

Other runtimes (e.g., [LangChain](https://docs.langchain.com/oss/python/integrations/providers/open_agent_spec))
may use different method names. The important part is that the
JSON defines the agent instructions, model placeholders, public tool names, and
public tool schemas; your hosting environment supplies the model binding, tool
implementations, database connection, and schema.

## Deploy The Spec

1. Configure a model.

   The exported JSON uses placeholders:

   ```json
   {
     "model_id": "MODEL_ID",
     "url": "MODEL_ENDPOINT"
   }
   ```

   Replace these with the model id and endpoint for your serving environment, or
   configure your Agent Spec loader to map them at deployment time. Use a model
   that can reliably write Oracle SQL and call tools.

2. Configure an Oracle connection.

   The tools expect a DB-API compatible Oracle connection factory: a callable
   that returns an Oracle connection with cursor support.

   Use a read-only database user where possible. The SQL execution tool sets the
   session transaction to read-only, but database privileges should still be the
   primary safety boundary.

3. Choose the target schema.

   Preconfigure the schema name once for the deployed agent. Your runtime should
   pass the target schema through hidden tool context.

4. Pass hidden tool arguments.

   The public tool signatures are small on purpose. At runtime, inject hidden
   arguments when constructing or executing tools:

   ```python
   hidden_args = {
       "connection_factory": connection_factory,
       "schema": "YOUR_SCHEMA",
   }
   ```

   `connection_factory` and `schema` are required.

5. Register the tools.

   The Analytic View spec exposes these tools:

   - `get_schema_overview()`: lists available tables/views and analytic views.
   - `get_table_schemas(tables: list[str])`: returns column metadata for base
     tables or views.
   - `get_analytic_view_ddls(analytic_views: list[str])`: returns full Oracle DDL
     for requested analytic views. Names are case-insensitive; `%` and `_` may be
     used as SQL LIKE wildcards.
   - `get_analytic_view_columns(analytic_view_name: str, grep: str | None = None)`:
     returns queryable analytic-view columns, roles, hierarchy aliases, level
     paths, and measure metadata.
   - `run_sql(sql_text: str, max_rows_fetch: int = 50)`: executes read-only
     Oracle SQL and returns a compact CSV preview.

   The vanilla spec uses only `get_schema_overview`, `get_table_schemas`, and
   `run_sql` from this list.

6. Run the agent.

   A typical interaction flow is:

   - Call `get_schema_overview()` to discover available objects.
   - Call `get_analytic_view_columns()` and/or `get_analytic_view_ddls()` for the
     relevant analytic view.
   - Use `run_sql()` to validate the final Oracle Analytic View query.
   - Return only the final SQL and result section required by your application.

The export does not inline schema metadata into the system prompt. Schema
discovery happens through `get_schema_overview()` at runtime, so importing the
JSON should not require live database access unless your runtime validates
connections while loading tools.

## Operational Notes

- Keep credentials out of the JSON spec. Inject them through your runtime's
  secret manager or environment configuration.
- Prefer a dedicated read-only Oracle user with access to the target schema and
  analytic-view metadata views such as `ALL_ANALYTIC_VIEWS`,
  `ALL_ANALYTIC_VIEW_COLUMNS`, `ALL_ANALYTIC_VIEW_HIERS`, and
  `ALL_ANALYTIC_VIEW_LEVELS`.
- If optional analytic-view metadata views are unavailable, tools should fail
  cleanly or return empty metadata rather than inventing schema details.
- The agent is designed for Oracle Analytic View SQL. Its system prompt expects
  queries to use `FROM ANALYTIC VIEW (USING ...)` patterns when analytic views
  are applicable.
