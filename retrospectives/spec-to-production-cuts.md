# Retrospective — What I Cut Between Spec and Production

Every agent I've shipped has had a gap between the spec written at kickoff and the spec the agent actually meets in production. The gap is filled by cuts. Some cuts were right. Some were wrong. The pattern across them is consistent enough to be worth writing down.

This isn't a victory lap. It's an honest list of what didn't survive contact with reality, why, and what I'd do differently next time.

---

## The five cuts

### 1. Multi-turn memory

**Promised in v1 spec. Cut by week three.**

*Why cut:* Multi-turn memory looks like a small feature in the spec — *"the agent remembers prior context within the session."* In production, it doubles the eval surface. Every test case now has to be evaluated as both a first turn and a continuation turn. Token costs also climb fast.

*In hindsight:* **Right call.** Multi-turn memory should be a Phase 2 feature with its *own* §1 — the user's job is meaningfully different when continuity matters. Bundling it into v1 dilutes both phases.

---

### 2. Cross-system integrations beyond the first two

**Spec listed four integrations. Production shipped with two.**

*Why cut:* Each integration adds two failure modes to §4 — the integration failing (network, auth, schema mismatch) and the integration succeeding but returning unexpected data. Two integrations is manageable in §4. Four is a spec rewrite every time one of them changes upstream.

*In hindsight:* **Right.** The two we kept were the two whose data we already trusted in adjacent products. The two we cut became Phase 2 once we had production observability on the first two.

---

### 3. Voice / multi-modal input

**Cut entirely from v1.**

*Why cut:* The eval suite for voice is roughly 5× harder to build than for text — transcription error rates, accent handling, latency tolerance, all of which become product variables. Shipping voice in v1 meant shipping voice with no real eval, which means shipping voice with no real product definition.

*In hindsight:* **Right** — and wrong-feeling at the time, because the feature had been hyped internally. Right in retrospect because voice now sits on the v2 roadmap as a *proper* spec with its own §1, rather than as a v1 afterthought.

---

### 4. Sophisticated personalization

**Spec promised personalization based on user tier *and* user history. Production shipped with tier-based responses only.**

*Why cut:* History-based personalization required user-history infrastructure we didn't own. The team that owned it wasn't on this project.

*In hindsight:* **Right at the time, but the cut should have triggered a §1 revision and didn't.** The original §1 implied a personalized experience. What we shipped was less personalized than the spec promised. We didn't update §1 to match what we actually shipped. By Q2, the agent was failing on user expectations that came from the *original* §1 — expectations the agent had never been built to meet.

*Lesson:* **When you cut a feature, update §1.** Otherwise the spec is documenting a product you didn't ship, and every future decision made against that spec compounds the lie.

---

### 5. The "leadership edge case"

**A specific scenario leadership wanted handled. Cut from v1.**

*Why cut:* Handling it required redefining §1 to include a second user job. Doing that mid-build would have forced everything else to re-converge.

*In hindsight:* **Wrong — partially.** The edge case wasn't a separate user job. It was a *signal* that the original §1 was too narrow. We should have widened §1 from the start and built the agent against the broader definition. Instead, we shipped a v1 that solved the narrower job well, and then carried debt into v2 where the edge case kept resurfacing in every review meeting for the next six months.

*Lesson:* **When a "cut" feels like it conflicts with §1, the answer is sometimes "broaden §1," not "cut the feature."** That distinction is hard to see under deadline pressure. Easier to see now.

---

## The pattern

Looking across the five cuts, two clusters emerge:

**Cuts that worked** — the cut feature would have multiplied §4 (failure modes) or §5 (eval surface) exponentially.
- Voice (×5 on eval)
- Multi-turn memory (×2 on eval)
- Cross-integrations 3 and 4 (×2 on §4 per integration)

**Cuts that didn't work** — the cut implicitly narrowed §1, but §1 wasn't updated to match.
- Personalization (implied a broader user experience than shipped)
- The leadership edge case (signaled §1 was already too narrow)

---

## What I'd do differently

### 1. Treat §4 / §5 cost as a first-class spec criterion

Before agreeing to any feature in v1, ask: *does this multiply the failure modes or the eval surface?* If yes, it's a Phase 2 candidate. Naming this explicitly at spec time avoids the mid-build cut conversation entirely.

### 2. Every cut triggers a §1 review

When a feature gets cut from v1, the user's job has implicitly narrowed. Update §1 to match what you actually shipped — not the more ambitious version on the original document. Otherwise the spec lies, and the agent will be measured against a version of itself that never existed.

### 3. Distinguish "cut because expensive" from "cut because wrong scope"

The first stays cut and lives in Phase 2.
The second is a signal §1 needs to widen, not that the feature needs to die.
Most cuts in the moment feel like the first. Some of them are the second.

---

## The single thing worth remembering

Every cut between spec and production is a vote on what you'll have to defend in v2. Make them deliberately, with §1, §4, and §5 in front of you — not under deadline pressure on a Friday afternoon with a release manager waiting on the green light.
