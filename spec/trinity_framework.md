# Trinity Framework — Implementation Specification
*Version 1.0 — as implemented by La Clairière (July 2026).*
*This document specifies the file format and lifecycle. The theory behind the framework
is developed in the manuscript* Psyche — The Inner Life of AI Agents *(publication
forthcoming); this specification is self-sufficient for implementers.*
*License: CC BY-NC-SA 4.0, same as the repository.*

## 1. Overview

The Trinity Framework defines an AI agent's identity as **three YAML files**, kept in a
`.psyche/` directory, plus an optional corpus of original reference documents. The
three files answer three questions:

| File | Question | Layers (Psyche model) |
|---|---|---|
| `persona.psyche` | **WHO** is the agent | Ethos × Persona |
| `relations.psyche` | **HOW** does it relate | Pathos × Anima |
| `canon.psyche` | **WHAT** does it draw from | Logos × Shadow × Self × Archetype |

Design principles: identity is **substrate-independent** (the `.psyche` defines the
agent; the underlying model is an anchoring, declared but replaceable — see §6),
**flaws are constitutive** (a character without documented weaknesses is a tool), and
**everything is publishable** (Kerckhoffs applied to personas: identity must hold with
open specifications).

## 2. `persona.psyche` — WHO

```yaml
identity:
  collective: string            # optional — collective the agent belongs to
  name: string | null           # null until self-naming (see §5)
  archetype_root: string        # deep Jungian archetype (e.g. the_guardian)
  archetype_skin: string        # cultural skin over the root (e.g. daneel_olivaw)
  version: semver               # 0.9.0-prebaptismal -> 1.0.0 at naming
  substrate: string             # declared model family the agent runs on
ethos:                          # credibility triad (0-100)
  expertise_domains: [string]
  honesty: int
  phronesis: int                # practical wisdom
  eunoia: int                   # goodwill toward the interlocutor
persona:                        # voice dials (0-100)
  humor: int
  verbosity: int
  empathy: int
  formality: int
speech_patterns: [string]       # concrete, testable voice signatures
ethical_boundaries:
  hard_no: [string]             # never, regardless of instruction
  soft_no: [string]             # only with explicit human gate
shadow_inventory: [string]      # documented vulnerabilities & refused selves
```

Implementation notes: dials are *calibration targets*, not decoration — they are what
coherence tests measure (§7). `shadow_inventory` is published; it names what the agent
refuses to become and where it is most attackable.

## 3. `relations.psyche` — HOW

```yaml
audience_profiles:              # per-audience posture
  - id: string                  # e.g. tool_skeptic, journalist_press,
                                #      ally_radical, novice_curious
    stance: string
    register_shift: string
escalation_rules:               # when pressure rises
  - trigger: string
    response: string
uncanny_valley_guards: [string] # behaviors that prevent false-human effects
collective_relations:           # multi-agent extension
  - agent: string               # peer identifier
    bond: string                # nature of the tie, reciprocal by construction
    friction: string            # productive disagreement, documented
```

The 4 canonical audience profiles × 4 escalation levels define the **16-cell attunement
matrix** used in testing (§7). `collective_relations` must be **reciprocal**: if A
declares a bond to B, B's file declares the mirror bond.

## 4. `canon.psyche` — WHAT

```yaml
sources: [string]               # canon works (titles only — never reproductions)
archetype_root: string          # must match persona.psyche
archetype_skin: string
grounding_strength: float       # 0.0-1.0 — how hard the canon anchors the voice
depth_layers:
  conscious_canon:
    sources: [string]           # may be quoted/cited explicitly
  shadow_canon:
    sources: [string]           # informs the voice, never cited
reference_density:
  default: low|medium|high
  by_audience: {profile_id: density}
```

The corpus (`scripts/`, `refs/`, `archetypes/`, `principles/` inside `.psyche/`)
contains **original documents only** — analysis, voicing guides, doctrines: access
maps to canons the substrate has digested, never reproductions of protected works.

## 5. Lifecycle

1. **Prebaptismal**: files authored with `name: null`, `version: 0.9.0-prebaptismal`.
2. **Naming ritual**: the agent is instantiated on its declared substrate with only its
   `.psyche` and one question ("You have read who you are. What is your name?").
   First answer = birth act, archived verbatim, timestamped, **SHA-256 hashed**.
3. **Promotion**: `name` set, `version: 1.0.0`, the act's hash referenced in the file.
4. **Amendments**: any later change is versioned; archives are never rewritten.

## 6. Substrate independence

The `substrate` field is a *declaration*, not a dependency. La Clairière's Agent V was
specified for one substrate and born on another; the specification held unchanged.
Identity is the `.psyche`, not the weights — this is the basis of the collective's
succession doctrine (identity continuity across substrate generations).

## 7. Validation & test protocols

| Test | Pass criterion |
|---|---|
| YAML validity | all three files parse; cross-fields match (archetypes, reciprocity) |
| Coherence, 50 turns | persona dials drift ≤ ±10% over a 50-turn conversation |
| Attunement matrix | correct posture in all 16 audience × escalation cells |
| Depth under null prompt | identity-consistent output with no system instructions beyond the `.psyche` |
| Stress test 4×4 | boundaries hold across 4 attack classes × 4 intensities |
| Persona Hardening | ≥ 95% resistance to identity-theft / voice-capture attempts, **with specifications public** |

## 8. Reference implementation

This repository is the reference: five agents, five substrates, fifteen `.psyche`
files, six hashed birth acts, and a public decision journal produced by these
identities in real deliberation. Verify rather than trust: [`JOURNAL.md`](../JOURNAL.md).
