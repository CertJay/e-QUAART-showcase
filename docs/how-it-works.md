# How e-QUAART works

[← Back to overview](../README.md)

## Data model, top to bottom

```
Division → District → School → School year / Term
   → Grade level → Class → Enrolment → Learner
   → Learning area → Assessment (items per competency)
   → Result (score / % / descriptor → level → tier) → Competency result (mastered?)
   → Learning gap → Intervention (baseline, sessions, attendance)
   → Reassessment → Effectiveness (computed)
```

A learner's history across years and schools is kept through enrolments, so their longitudinal view is derived, not stored twice.

## Classification

- **Percentage instruments** (for example the end-of-term LOA): `percentage = score ÷ max × 100`, and the result takes the highest band whose minimum it reaches.
- **Profile instruments** (CRLA, Phil-IRI, RMA): the teacher records the descriptor, and it maps directly to a level and tier. No percentage rule is applied.
- **Competency mastery**: items correct ÷ items total, against a configurable threshold (default 75%).
- **Band sets are validated before saving**: they must start at 0%, must not overlap, and descriptor keys must be unique. Editing a band set re-classifies existing results and re-derives gaps in one audited transaction.

## From results to gaps

When an assessment is submitted, gaps are derived automatically:

- one gap for each competency a learner did not master;
- one learning-area gap for each Tier 2 or Tier 3 result that has no competency breakdown.

Gaps move from **Open** to **In intervention** to **Resolved**, and a gap resolves when a reassessment reaches Tier 1.

Group patterns are computed on the fly, with configurable thresholds:

| Pattern | Rule (defaults) |
|---|---|
| Class-level gap | 40% or more of a class did not master the competency |
| School-level gap | 2 or more classes in a school share it |
| Division-level gap | 2 or more schools share it |
| Persistent gap | a grade and learning area stays above the at-risk alert rate for consecutive terms |

## Metric definitions

Every dashboard and report uses these, so a metric means the same thing on every screen.

| Metric | Definition |
|---|---|
| Proficiency rate | Tier 1 results ÷ classified results |
| At-risk rate | Tier 2 + Tier 3 results ÷ classified results |
| Completion | results encoded (including absent) ÷ learners enrolled in the assessed classes |
| Intervention coverage | learners with a gap who are in a matching intervention ÷ learners with a gap |
| Improvement rate | reassessed learners whose score or level improved ÷ reassessed learners |
| Effectiveness (per learner) | score change, % improvement and level change, with the options the data points to (continue, modify, complete, repeat, refer). The teacher decides. |

Only submitted and verified assessments feed analytics, and any view can be limited to verified results.

## Testing

Unit and integration tests run against a seeded, fictional dataset on every push, together with type checks. They cover, among other things:

- tiers computed from the configured bands, and profile instruments never classified by percentage;
- verified results locked, and every edit audit-logged;
- a request outside a user's scope refused;
- division-level gaps detected across two or more schools;
- an intervention going from gap to session to reassessment to an effectiveness result;
- exports audit-logged, with small cells suppressed.
