---
name: project-planning
description: >-
  Plan and triage Grafana work using the Beeson Jira board and Beeson-Grafana
  Figma file. Use when the user asks about issues, tickets, bugs, tasks,
  epics, sprints, backlog, designs, mockups, specs, UI, wireframes, Figma,
  Jira, planning, or implementation scope for this project.
---

# Project planning: Jira + Figma

## Canonical sources

Always scope project-planning work to these sources. Do not search other Jira
projects/boards or Figma files unless the user explicitly asks.

### Jira

| Field | Value |
| --- | --- |
| Board | Beeson \| Grafana |
| Project key | `BGRAF` |

- Use Atlassian MCP tools for all Jira reads and writes.
- Scope JQL to `project = BGRAF` unless the user gives a specific issue key
  (e.g. `BGRAF-123`).
- When listing board or sprint work, target **Beeson \| Grafana** — not other
  boards or projects.
- Prefer board/sprint context over org-wide Jira search.

**JQL examples:**

```
project = BGRAF ORDER BY updated DESC
project = BGRAF AND status != Done ORDER BY priority DESC
project = BGRAF AND sprint in openSprints()
project = BGRAF AND text ~ "search terms"
```

### Figma

| Field | Value |
| --- | --- |
| File | Beeson-Grafana |
| URL | https://www.figma.com/design/Xq8tlIFyHGEIxeLdnTr0dl/Beeson-Grafana?node-id=0-1 |
| fileKey | `Xq8tlIFyHGEIxeLdnTr0dl` |
| Default nodeId | `0:1` (file root; from `node-id=0-1`) |

- Use Figma MCP tools for all design reads and design-to-code work.
- Default to this file and fileKey for every design query.
- Use `get_design_context` as the primary read tool; pass the fileKey above.
- For file-level exploration, use nodeId `0:1`. For a specific screen or
  component, use the node id from the user's Figma URL (`-` → `:`).
- Before `use_figma`, read `/figma-use` (skill://figma/figma-use/SKILL.md).

## When to apply

Apply this skill when the user asks about any of:

- Issues, tickets, bugs, tasks, epics, stories, backlog, sprint, or Jira
- Designs, mockups, wireframes, UI specs, or Figma
- Planning, scoping, or prioritizing work for this Grafana effort
- Linking implementation to design or ticket context

## Workflow

### 1. Classify the request

| User need | Primary source | Action |
| --- | --- | --- |
| Ticket status, backlog, sprint, bugs | Jira | Query `BGRAF` via Atlassian MCP |
| Visual design, layout, components | Figma | Read Beeson-Grafana via Figma MCP |
| Implement a feature | Both | Pull ticket + linked/relevant frames |
| Plan or prioritize | Both | Summarize open `BGRAF` work and design coverage |

### 2. Gather context

**Jira**

1. Resolve `cloudId` if needed (Atlassian MCP).
2. Search or fetch with JQL scoped to `project = BGRAF`.
3. Cite issue keys (e.g. `BGRAF-42`) in responses.

**Figma**

1. Use fileKey `Xq8tlIFyHGEIxeLdnTr0dl`.
2. Call `get_design_context` (or `get_metadata` / `get_screenshot` when
   appropriate) with the relevant nodeId.
3. Cross-reference ticket summaries or acceptance criteria when both exist.

### 3. Synthesize for planning

When producing plans, status, or implementation guidance:

1. **Tickets** — key, summary, status, assignee, priority, relevant AC/description.
2. **Design** — frame names, screenshots or structure, gaps vs. tickets.
3. **Recommendations** — next steps, blockers, missing design or ticket coverage.

Keep output actionable. Prefer tables or checklists over long prose.

## Rules

- **Jira**: Beeson \| Grafana (`BGRAF`) only unless the user overrides.
- **Figma**: Beeson-Grafana file only unless the user overrides.
- **Combined queries**: Always check both sources when the question spans
  product, design, and engineering.
- **Writes**: Do not create, update, or transition Jira issues or edit Figma
  unless the user explicitly asks.
- **GitHub**: For PR/issue work on code, also follow
  [github-fieldsphere-fork](../github-fieldsphere-fork/SKILL.md).

## Examples

**"What's in the current sprint?"**

→ Jira: `project = BGRAF AND sprint in openSprints()` on board Beeson \| Grafana.

**"Show the dashboard design"**

→ Figma: `get_design_context` with fileKey `Xq8tlIFyHGEIxeLdnTr0dl`; use the
node id from the user's URL if they link a specific frame.

**"Plan work for the alerting redesign"**

→ Jira: `project = BGRAF AND text ~ "alerting"`  
→ Figma: explore Beeson-Grafana for alerting-related frames  
→ Return a combined plan with ticket ↔ design mapping.
