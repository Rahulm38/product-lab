---
name: diagram-design
description: Design clear technical, product, process, architecture, data, timeline, state, and comparison diagrams with an explicit information hierarchy and reusable visual grammar.
---

# Diagram Design

Turn complex information into a diagram that answers one clear question.

The goal is not decoration. A good diagram makes relationships, sequence, ownership, state, or comparison easier to understand than prose.

## Start with the question

Before choosing a visual form, state the question the diagram must answer.

Examples:

- What happens from user action to final system outcome?
- Which services own each stage of a request?
- How does an entity move between states?
- What changed before and after a release?
- Which initiatives depend on which capabilities?
- Where does data enter, transform, persist, and leave?

If the diagram tries to answer several unrelated questions, split it.

## Choose the structure

Use the simplest form that matches the information.

| Need | Good default |
| --- | --- |
| Ordered decisions or actions | Flowchart |
| Interactions across actors over time | Sequence diagram |
| Responsibility across teams/systems | Swimlane |
| Allowed states and transitions | State diagram |
| System components and boundaries | Architecture diagram |
| Movement and transformation of data | Data-flow diagram |
| Parent-child structure | Tree |
| Nested ownership or containment | Nested boxes |
| Layers or maturity levels | Layer diagram |
| Dates and milestones | Timeline / Gantt |
| Comparison on two dimensions | Quadrant / scatter |
| Part-to-whole hierarchy | Treemap |
| Overlap | Venn |
| Circular repeated behavior | Loop |
| Database entities and relationships | ER diagram |

Do not force a popular diagram type when another form communicates the relationship better.

## Build the information hierarchy

Use three levels at most in the first view:

1. **Primary path or conclusion** — what the reader should notice first.
2. **Supporting structure** — actors, systems, stages, or categories.
3. **Secondary detail** — exceptions, annotations, metrics, or notes.

Move implementation trivia into labels, notes, or a second diagram rather than shrinking everything to fit.

## Visual grammar

Keep a consistent meaning for visual properties:

- position = order, ownership, or hierarchy;
- connector direction = flow or dependency;
- line style = normal, optional, asynchronous, or exceptional path;
- shape = role or object type;
- emphasis = the small number of elements that matter most.

Do not use color as the only carrier of meaning. Labels and shapes should still work in grayscale and for readers with color-vision differences.

## Layout rules

- Prefer left-to-right for flows and sequences unless the medium strongly favors vertical reading.
- Keep the primary path visually straight.
- Minimize crossing connectors.
- Align related nodes to a grid.
- Keep spacing consistent.
- Put labels close to the objects they describe.
- Keep boundary boxes visually quieter than the content inside them.
- Avoid tiny text and excessive legend dependence.
- Use whitespace to separate concepts instead of decorative boxes everywhere.

## Text rules

Use short noun or verb phrases inside shapes.

Prefer:

```text
Validate request
Fetch profile
Persist result
Retry available
```

Avoid sentences unless the text is an annotation or decision condition.

For decision branches, label connectors with the condition rather than putting both outcomes in a paragraph inside the diamond.

## Technical diagrams

For architecture, integration, or data-flow diagrams:

- show trust/system boundaries explicitly;
- distinguish synchronous from asynchronous calls when important;
- show stores separately from services;
- identify external systems;
- show the direction of data movement;
- annotate protocol or contract only when it changes understanding;
- do not invent unverified services, queues, databases, or ownership.

When source evidence is incomplete, label uncertain components as proposed or needs verification.

## Product and process diagrams

For user journeys, operational flows, and service blueprints:

- keep user intent visible;
- separate user action, system reaction, and operational/manual work;
- show negative and recovery paths when they affect the experience;
- avoid turning a product journey into an implementation diagram unless implementation is the question.

## State diagrams

Define:

- valid states;
- event/condition causing each transition;
- terminal states;
- retry or rollback transitions;
- impossible or explicitly disallowed transitions when relevant.

Do not infer a transition merely because two states exist in code.

## Diagram output

When producing code-based diagrams:

- keep source editable;
- use stable node IDs;
- keep presentation separate from data where practical;
- add a title that states the question or scope;
- include a short legend only when the visual grammar is not self-evident.

When producing an image, also retain a text or source representation when the workflow supports it so the diagram can be updated later.

## Review checklist

Before handoff, verify:

1. The diagram answers one clear question.
2. The primary path is obvious in a few seconds.
3. Every node earns its place.
4. Connectors have unambiguous direction.
5. Labels are readable at normal viewing size.
6. The same shape/line treatment means the same thing throughout.
7. Edge and failure paths are included only when relevant.
8. No visual element implies an unverified fact.
9. The diagram still makes sense without relying on color alone.
10. The output is editable or reproducible where possible.

## Handoff

Report the diagram's purpose, source evidence, important assumptions or unknowns, chosen diagram type, output location/format, and any deliberately omitted detail.
