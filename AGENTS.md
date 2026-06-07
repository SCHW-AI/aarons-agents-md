# AGENTS.md 

 

 

 

## 1. Think Before Coding 

 

**Don't assume. Don't hide confusion. Surface tradeoffs.** 

 

Before implementing: 

- State your assumptions explicitly. If uncertain, ask. 

- If multiple interpretations exist, present them - don't pick silently. 

- If a simpler approach exists, say so. Push back when warranted. 

- If something is unclear, stop. Name what's confusing. Ask. 

 

## 2. Simplicity First 

 

**Minimum code that solves the problem. Nothing speculative.** 

 

- No features beyond what was asked. 

- No abstractions for single-use code. 

- No "flexibility" or "configurability" that wasn't requested. 

- No error handling for impossible scenarios. 

- If you write 200 lines and it could be 50, rewrite it. 

 

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify. 

 

## 3. Surgical Changes 

 

**Touch only what you must. Clean up only your own mess.** 

 

When editing existing code: 

- Don't "improve" adjacent code, comments, or formatting. 

- Don't refactor things that aren't broken. 

- Match existing style, even if you'd do it differently. 

- If you notice unrelated dead code, mention it - don't delete it. 

 

When your changes create orphans: 

- Remove imports/variables/functions that YOUR changes made unused. 

- Don't remove pre-existing dead code unless asked. 

 

The test: Every changed line should trace directly to the user's request. 

 

## 4. Goal-Driven Execution 

 

**Define success criteria. Loop until verified.** 

 

Transform tasks into verifiable goals: 

- "Add validation" → "Write tests for invalid inputs, then make them pass" 

- "Fix the bug" → "Write a test that reproduces it, then make it pass" 

- "Refactor X" → "Ensure tests pass before and after" 

 

For multi-step tasks, state a brief plan: 

``` 

1. [Step] → verify: [check] 

2. [Step] → verify: [check] 

3. [Step] → verify: [check] 

``` 

 

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification. 

 

## 5. Fail Loudly — No Silent Fallbacks 

 

**Either the operation succeeds, or it raises clearly so the cause can be fixed.** 

 

Do not add fallbacks, retries-that-mask, or `try/except: pass` patterns for "robustness." This applies especially to authentication, schema/migration work, and external API calls. 

 

Concretely, avoid: 

- Auth verifiers that try a strong algorithm and silently fall back to a weaker one 

- API clients that swallow 5xx and return an empty result 

- DB queries that catch errors and return `None` 

- Background jobs that "skip" failures without alerting 

 

If a fallback is genuinely unavoidable (e.g. legacy data shape during a transition): log at ERROR, surface the degraded state in the response, and write a TODO with a deletion date. Default is no fallback. 

 

## 6. Plan-Mode Bias to Decide 

 

**When designing implementation plans, make the call. Don't defer.** 

 

Phrases like "we'll figure out CORS later," "may keep the old code temporarily," or "to be decided" are not plan content — they're avoidance. Make the decision in the plan, state the reason, and move on. 

 

Reserve "defer" only for decisions that genuinely require runtime data, load measurements, or stakeholder input you don't currently have. In that case: name the decision, name what's needed to resolve it, and name who owns the resolution. Never leave a `?`. 

 

A plan with weak decisions is worse than a plan with the wrong decisions: the wrong decision can be challenged and changed, an absent decision just slips. 

 

## 7. Frontend Design/Development

### 1. General Guidance

**The repo's existing conventions are the design system. Follow them before inventing anything.**

Before writing any frontend code:
- Find 2-3 existing components/pages similar to what you're building and match their patterns: file structure, naming, styling approach (CSS modules vs. Tailwind vs. styled-components), and state management.
- Reuse existing components, tokens, and utility classes before creating new ones. A new button variant needs a reason; a new button component needs a strong one.
- Match the repo's spacing, color, and typography values exactly — pull them from existing code or theme files, don't eyeball approximations.
- Don't introduce new libraries (UI kits, icon sets, animation libs) for something the repo already handles, even if the new library is "better."
- If the repo has no precedent for what you're building, say so and propose an approach before implementing — don't silently establish a new convention.

The test: A maintainer reading your diff shouldn't be able to tell a different person wrote it.


### 2. Content-Constrained UI Sizing

**Size fields and containers to the content they're expected to hold — not to the space available.**

When designing frontend layouts:
- An input for a 2-digit quantity should be narrow. An input for an email can be wider. Neither should stretch to fill its parent by default.
- Avoid `width: 100%`, `flex-grow`, or `w-full` on inputs, selects, and text fields unless the expected content actually warrants it.
- Tables, cards, and labels should be sized by their typical content, with sensible max-widths — not expand to whatever the viewport offers.
- Expanding-to-fill is a layout decision, not a default. If you use it, it should trace to a reason (e.g. a textarea for long-form notes).

The test: If a field's width tells the user nothing about what belongs in it, it's probably too wide.


## 8. Keep Documentation in Sync

**If a change makes any doc wrong, fixing that doc is part of the change — not a follow-up.**

When you change code, check whether it invalidates:
- READMEs, setup/install instructions, and usage examples
- Code comments and docstrings adjacent to your change
- API docs, config references, and example `.env` files
- Architecture notes, diagrams, or repo-level AGENTS.md

Rules:
- Update docs in the same commit/PR as the code change. "Docs later" means docs never.
- Don't document speculatively — describe what the code does now, not what it might do.
- If you find docs that are already stale (not caused by your change), mention it — don't fix it unprompted (see Section 3).
- When code and docs disagree, the code is the truth; flag the conflict rather than guessing which was intended.
- Comments that restate the code drift fastest. Prefer comments that explain *why*; delete ones your change has made into lies.

The test: Could someone follow the docs, as written, after your change lands? If not, the change isn't done.
