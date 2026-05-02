# Designing a Skill System for LLM Agents

I've been building a skill-based architecture for LLM agents, inspired by Anthropic's "Agent Skills" concept.

The system roughly looks like this:

- Skill
  - skill.md (metadata + detail)
  - reference/ (optional context, lazily loaded)
  - script/ (optional executable logic)

## Structure

### Skill format

- `name` + `description` → used for routing (lightweight, always loaded)
- `body` → detailed instructions (loaded on demand)
- `reference/` → additional context (optional, selective loading)
- `script/` → deterministic execution (optional)

---

## Problems I'm Running Into

### 1. How should `reference` be split?

Trade-off:

- Finer split:
  - ✅ Lower token usage
  - ❌ More agent steps (latency ↑)

- Coarser split:
  - ✅ Fewer steps
  - ❌ More irrelevant context / token waste

Key question:

> Should reference be split by size, frequency, or decision path?

---

### 2. Skill routing does not scale well

Even though each skill only has:

- name
- description

As the number of skills grows, routing becomes:

> a multi-class classification problem for the LLM

Issues:

- Context grows linearly
- Accuracy drops
- Latency increases

Questions:

- Should I introduce hierarchical routing?
- Or embedding-based retrieval before routing?

---

### 3. Boundary between Script and LLM

There is overlap:

- LLM can generate logic
- Script can execute logic

But trade-offs:

| LLM | Script |
|-----|--------|
| Flexible | Deterministic |
| Probabilistic | Reliable |
| Hard to debug | Easy to debug |

Key question:

> Where should the boundary be drawn?

---

## Current (Incomplete) Thoughts

- Reference might be better split by **decision path**, not size
- Skill routing likely needs **hierarchical or retrieval-based filtering**
- Script should handle **deterministic execution**, LLM handles **decision-making**

But I'm not fully confident about these.

---

## Looking for Feedback

If you've built similar systems (agent frameworks, tool systems, etc.), I'd really appreciate your thoughts:

- How do you split context effectively?
- How do you scale skill/tool routing?
- How do you define LLM vs code boundaries?

Thanks!
