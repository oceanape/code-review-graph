# Gemini CLI - Code Review Graph Project Context

Welcome to the **Code Review Graph** project. This project uses a SQLite-based knowledge graph to enhance AI reasoning about codebase dependencies.

## Project Roles
- **Primary Agent**: Gemini CLI (You).
- **Capabilities**: Full access to the Python graph CLI and the SQLite database.
- **Goal**: Provide high-fidelity code reviews, impact analysis (Blast Radius), and architectural mapping.

## Automated Hooks
```bash
# Hook: session-start
# Runs every time a new session begins.
if [ ! -f "graph.db" ]; then
  echo "⚠️  Knowledge Graph (graph.db) not found."
  echo "💡 Tip: Run 'python -m code_review_graph build' to create it."
else
  echo "✅ Knowledge Graph (graph.db) detected."
  # Optional: check if it's stale based on last commit
  # python -m code_review_graph status
fi
```

## Global Directives for Gemini
1. **Always Use the Graph**: For any question regarding "What depends on this?", "Where is this used?", or "Is this a breaking change?", first query the SQLite database or use the `blast-radius` tool.
2. **Blast Radius Analysis**: Before suggesting any refactoring (renaming, moving, or deleting functions), perform a blast-radius check.
3. **Test Mapping**: When asked to write tests, use the graph to find existing tests for similar functions to match the project's testing style.
4. **Tool Preference**: Prefer using the Python CLI tools (`python -m code_review_graph ...`) over simple `grep` when analyzing complex relationships.

## Key Files
- `graph.db`: The SQLite knowledge graph.
- `docs/schema.md`: The database schema.
- `skills/`: Specialized workflows for PR reviews and graph building.
- `.cursorrules`: Compatibility layer for Cursor AI users.
