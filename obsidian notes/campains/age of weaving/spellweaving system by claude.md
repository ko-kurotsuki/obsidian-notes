Here's a framework that should let you convert basically any 5e spell into your thread-cost system without needing a custom ruling every time.

**Core formula**

Every spell's thread cost = Effect Points (what the spell _does_) modified by Principle Modifiers (how it's delivered). The weaving test DC stays 10 + final thread cost, same as your existing rule.

**Effect Points (base cost of "what the spell does")**

This is the foundation, calibrated so a "typical" spell of level L costs roughly (2 × L) + 1 thread:

Damage/Healing: 1 point per d6 of average damage or healing (so 2d6 = 1 point, since avg ~7; round to nearest). A flat numeric bonus (like Guidance's +1d4) counts as half its die value.

Conditions: 1 point per round of a strong condition (paralyzed, stunned, incapacitated), or 1 point for the _first_ minute of a weaker condition (charmed, frightened, restrained) plus 1 point per additional minute.

Buffs/debuffs to stats: 1 point per +1/-1 to AC, attack rolls, saves, or ability checks, per minute of duration (minimum 1).

Movement/utility effects (flight, invisibility, telepathy, etc.): 2-4 points flat depending on how strong the effect is relative to its level — use the closest existing 5e spell of similar power as your anchor.

Summoning/creation: 1 point per CR (or per CR-equivalent of stat block) of the creature/object summoned.

**Principle Modifiers (how the effect is delivered)**

These add or subtract from the Effect Point total. Negative principles you don't know yet just can't be applied — same as your existing rule that you can't _use_ a principle from a school you don't possess, even if you've learned it.

_Range_

- Self: -1
- Touch: 0
- 30 ft: 0
- 60-120 ft: +1
- Sight/unlimited: +2
- Area effect (cone/sphere/line, standard sizes like 15-30ft): +1, regardless of how many creatures it can hit — the "efficiency" of AoE is the tradeoff (allies can be hit, saves apply)

_Weaving Time (casting time)_

- 1 action: 0
- Bonus action: +1
- Reaction: +1
- 1 minute or longer: -1 per "step" slower (1 min, 10 min, 1 hour, etc.)
- Ritual tag available: -1 (only usable outside combat)

_Duration_

- Instantaneous: 0
- 1 round/until next turn: 0
- 1 minute (no concentration): +1
- Concentration, up to 10 min: 0 (concentration is its own cost — see below)
- Concentration, up to 1 hour+: +1
- Non-concentration, 1 hour+: +2
- Permanent/until dispelled: +3

_Concentration as a principle_: rather than a binary tag, let concentration itself reduce cost by 1 (representing "you're actively feeding it thread"), but the spell collapses if concentration breaks — no thread refund.

_Components_

- V/S only: 0
- Material component, no cost listed: -1 (you found a small efficiency)
- Material component with a gp cost: -1 additional per order of magnitude of cost (25gp = -1, 250gp = -2, etc.), reflecting that you're literally feeding value into the weave instead of thread
- Focus/component you destroy or consume: -1 additional

_Scalability ("at higher levels")_

- Each additional "step" of effect (extra d6, extra target, +1 round duration, etc.) that a spell can gain when upcast costs +1 point _per step available_, even if the player doesn't use all the steps. This represents the spell's _potential_ — a spell built with more scalability headroom costs more thread even at minimum effect, but lets the caster pour extra thread in later.

_Miscellaneous_

- Multiple distinct effects bundled into one spell (e.g., damage + a condition): each additional effect bundled costs +1 on top of its own Effect Point cost (a "bundling tax").
- Affecting multiple separate targets (not an AoE template, but e.g. "up to 3 creatures you choose"): +1 per additional target beyond the first.

**Worked examples**

_Cure Wounds_: Healing 1d8+mod (~1 point), touch (0), 1 action (0), V/S (0), instantaneous (0). Total: 1 thread. Matches its 1st-level slot.

_Fireball_: Damage 8d6 (~4 points), sight range AoE (+1), 1 action (0), V/S/M no-cost-component (-1), instantaneous (0). Total: 4 thread. A bit pricier than its 3rd-level slot — appropriate, since in your system fire-aligned casters would discount it via planar alignment, which 5e doesn't model.

_Haste_: +2 AC and double speed/extra action for 1 minute, concentration, single target. Buff-to-stats (~2 points for the AC/action package, treat "extra action" as a strong effect ~2 points), concentration discount (-1), touch (0), V/S/M (-1). Total: ~2 thread, with the well-known drawback (lethargy after) representing the "discount."

_Wish_: Treat as Miscellaneous — effectively "spend thread equal to your maximum capacity, plus take the canonical risk (losing the ability to cast Wish again, stress damage)." This and a handful of other reality-warping spells should probably stay as hard-coded outliers rather than fitting the point system, same way 5e treats them as exceptions to its own rules.

aarcane gate
6th level means it should have 13 spell thread for it
1 action= 0
500ft range 10ft radius circle potral= 3
concentration up to 10 min=0

**A note on calibration**

These numbers will need adjusting once you see them in play — the goal of "2×level+1 ≈ typical thread cost" is a starting anchor, not gospel. I'd suggest running 4-5 spells your players actually want to build through this, see if the costs feel right relative to your spell thread pools (especially since half/third weavers have less thread to spend), and adjust the per-point values rather than the structure. The structure (Effect Points × Principle Modifiers) should hold up; the exact numbers are the dial you'll turn.

Want me to also build out a quick reference sheet/table format for this so it's easier to use at the table, or work through how this interacts with your planar alignment discount (e.g., a fire-aligned caster's Fireball costing less)?