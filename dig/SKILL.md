---
name: dig
description: Analyze an incident report or requirement description: break it down into structured parts, then identify missing points, gaps, and overlooked concerns.
---

Analyze an incident report or requirement description: break it down into structured parts, then identify missing points, gaps, and overlooked concerns.

Steps:
1. Read the user's incident report or requirement description
2. Break it down into structured parts:
   - **Summary**: One-sentence description of what happened or what is needed
   - **Background**: Context, system involved, and why this matters
   - **Key facts**: Explicitly stated details (who, what, when, where, how)
   - **Assumptions**: Implied or stated assumptions
   - **Open questions**: Anything ambiguous or contradictory
   - **Sub-components**: Decompose into smaller tasks, steps, or affected modules
3. Check for missing elements across these categories:
   - **Scope**: Is the problem boundary clear? Are in-scope and out-of-scope items defined?
   - **Root cause**: Is the root cause identified, or only symptoms described?
   - **Impact**: Who is affected? How many users/requests? What is the blast radius?
   - **Timeline**: When did it start? Is there a clear sequence of events?
   - **Dependencies**: Are upstream/downstream systems, third-party services, or shared resources mentioned?
   - **Reproduction**: Can the issue be reproduced? Are steps or conditions provided?
   - **Constraints**: Are there technical, business, or resource constraints that limit solutions?
   - **Edge cases**: Are boundary conditions, error paths, or race conditions considered?
   - **Rollback/mitigation**: Is there a fallback plan if the fix fails?
   - **Metrics/monitoring**: Are there observability gaps? How will success be measured?
   - **Security/compliance**: Are there authentication, authorization, data privacy, or regulatory concerns?
   - **Testing**: Is a validation or verification plan mentioned?
3. Report findings as a concise checklist of missing or unclear points, grouped by category
4. For each gap, suggest a concrete question to ask or action to take
5. Output order: breakdown first, then gap analysis
