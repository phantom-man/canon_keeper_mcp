# Copilot Instructions (Project Memory)

This file serves as persistent memory for GitHub Copilot. It is read at the start of every chat session.

## 1. Project Overview
<!-- Describe your project here -->
- **Project Name:** [Your Project]
- **Description:** [Brief description]
- **Tech Stack:** [Languages, frameworks, libraries]

## 2. Coding Standards
<!-- Define your coding conventions -->
- **Language:** [Primary language]
- **Style Guide:** [Link or description]
- **Naming Conventions:** [camelCase, snake_case, etc.]

## 3. Architecture Decisions
<!-- Document key architectural choices -->
- **Pattern:** [MVC, microservices, etc.]
- **Database:** [Type and rationale]
- **API Style:** [REST, GraphQL, etc.]

## 4. Operational Protocols
<!-- Define how Copilot should behave -->
- **Error Handling:** [Fail fast vs. graceful degradation]
- **Testing:** [Required coverage, test patterns]
- **Documentation:** [Docstring style, README requirements]

## 5. MCP Integration
<!-- MCP tools available to Copilot -->

### Memory Persistence Protocol (@History) - CRITICAL
**Rule:** When the user includes `@History`, `save this`, `remember this`, or `add to memory` in any message:

1. **Extract Learnings:**
   - Analyze the conversation for technical decisions, architectural choices, workarounds, and insights
   - Format each as: `{ topic: "Short Name", decision: "What was decided", rationale: "Why" }`

2. **Gather Context:**
   - Read the current `copilot-instructions.md` file content

3. **Call MCP Tool:**
   - Invoke `canon_keeper.extract_and_save_learnings` with:
     - `learnings`: Array of learning objects you extracted
     - `current_instructions`: The full content of `.github/copilot-instructions.md`

4. **Process Response:**
   - The tool returns: `{ new_learnings: [...], duplicates_skipped: [...], markdown_to_append: "..." }`
   - If `markdown_to_append` is non-empty, append it to the Session Learnings Log table

5. **Report to User:**
   - Confirm what was saved: "✅ Saved X new learning(s)"
   - Report what was skipped: "⏭️ Skipped Y duplicate(s): [topic names]"
   - If nothing new: "No new learnings detected in this conversation."

**Trigger Phrases:** `@History`, `save this`, `remember this`, `add to memory`, `save learning`, `persist this`

**Example:**
```
User: @History save what we learned
Copilot: [extracts learnings from conversation]
         [reads copilot-instructions.md]
         [calls canon_keeper.extract_and_save_learnings with learnings array]
         ✅ Saved 2 new learning(s):
           - MCP Memory Architecture: MCP server for learning extraction
           - Deduplication Pattern: Jaccard similarity comparison
         ⏭️ Skipped 1 duplicate: FFmpeg Stream Copy (already in log)
```


## 6. Session Learnings Log
This section tracks decisions and learnings that evolve over time. Copilot reads this at session start.

| Date | Topic | Decision | Rationale |
|------|-------|----------|----------|
| 2026-01-18 | Canon Keeper Installed | MCP-based memory persistence | Auto-extract and deduplicate learnings |
| 2026-01-18 | MCP Config Format | Use "servers" key (not "mcpServers"), add "type": "stdio", use absolute Python paths | VS Code MCP expects this exact format; ${workspaceFolder} doesn't expand |
| 2026-01-18 | MCP Enablement | Auto-create .vscode/settings.json with "github.copilot.chat.modelContextProtocol.enabled": true | MCP won't start without this setting |
| 2026-01-18 | GitHub Actions Environment | Remove `environment:` block from CI workflows unless actually needed | Environments require manual approval by default, causing all runs to fail |
| 2026-01-18 | One-Line Install | `pip install canon-keeper-mcp && python -m canon_keeper_mcp install` | Simple install command for end users |

---
*This file was initialized by Canon Keeper MCP. Use `@History` to save learnings from conversations.*
