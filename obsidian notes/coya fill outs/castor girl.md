---
des: 1
pro: 3
spi: 3
tra: 5
men: 2
con: 2
---

# Warlock
Casting trait: Charge caster
A warlock cannot cast spells unless they have gathered charges to do so through specific actions of devotion which are listed under their selected patron.
Minor charges cover spell from capacity 1 to 4, significant cover from 5 to 8, and capacity 9 and 10 spells require a major charge. All warlock spells are cast instantly.

Personal Trait: Patron
A warlock must select a patron. They gain the warlock bonus for that patron.

The Daughter of Lilith
The queen of succubi is a surprisingly common patron. Her desire is only to increase her own personal power... and the easiest way for her to do that is to increase the number of mortals doing the deed - promoting lust and providing a feeding ground for the succubi under her command. As such, all of her "gifts" are designed to futher that goal.

Trait bonus: Charm
Your caster girl is now able to cause a target to develop both a crush and an insatiable lust for someone that your caster girl specifies. Once the target has sex with the specified individual, the effect gradually fades.

Drawback effect Sluttification
Your caster girl is entirely unable to refuse sexual advances, regardless of who they come from.

Warlock bonus: Sexual prowess
Your caster girl becomes supernaturally good at sex, leagues beyond what normal mortals can aquire regardless of how much experience they get. She can play the human body's pleasure centres like a finely tuned instrument.
Warlock minor charge: Masturbation until completion.
Warlock significant charge: Having sex for at least one hour.
Warlock major charge: Take part in an orgy of at least three beings other than herself.

Des: 1
Pro: 1
Spi: 2
Tra:2 
Men:1
Con:2

```meta-bind-button
label: "+1"
hidden: true
id: "des-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: des
    evaluate: true
    value: "x + 1"
```
# body
Busty (Tra)
Short (Pro)
```meta-bind-button
label: "-1"
hidden: true
id: "des-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: des
    evaluate: true
    value: "x - 1"
```
Plush (Spi)
Occultish (Tra)


# personality
Caring (Pro)
Lewd (Tra)
Scholarly (Men)

# traits

 boons
Wealthy
Your caster girl is particularly wealthy in some fashion. Belonging to a stupidly rich family, being a previous lottery winner, owning highly successful business ventures. The two of you will never have financial problems.
Cross class ( summoner)
Your caster girl may have one additional personal trait from any other class.

Personal trait: Harem maker
Magic or no, a summoner knows how to ingratiate herself to (or manipulate) others. Within a short span of time she will gather a harem that she's willing to share with you.
You can specify the types of harem members you want, though this will make it take much longer to gather said harem.

Patron
One of the patrons has taken a shine to your caster girl... or owed her a favour. Select a patron (if other options make you select a patron, then it must be the same patron as all other options), your caster girl gains the trait benefit of that patron.
Trait bonus: Charm
Your caster girl is now able to cause a target to develop both a crush and an insatiable lust for someone that your caster girl specifies. Once the target has sex with the specified individual, the effect gradually fades.

Planeswalker
(cannot be druid)
Your caster girl can open gateways between your world and others, no longer limited to her native world. You can travel to any fictional world but each reality can only support its own rules plus your caster girl's magic... so bringing back items or skills that rely on things that don't exist in your world becomes pointless.

drawbacks
Weak
(cannot be adept)
For all her mystic might, your caster girl has zero physical aptitude. Poor manual dexterity, nearly as weak as a kitten and takes a week or so to get over even light colds.

```meta-bind-button
label: "Reset"
hidden: true
id: "des-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: des
    evaluate: false
    value: 0
```
Current des `VIEW[{des}]`

`BUTTON[des-decrement, des-increment, des-reset]`

```meta-bind-button
label: "+1"
hidden: true
id: "pro-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: pro
    evaluate: true
    value: "x + 1"
```

```meta-bind-button
label: "-1"
hidden: true
id: "pro-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: pro
    evaluate: true
    value: "x - 1"
```

```meta-bind-button
label: "Reset"
hidden: true
id: "pro-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: pro
    evaluate: false
    value: 0
```
Current pro `VIEW[{pro}]`

`BUTTON[pro-decrement, pro-increment, pro-reset]`

```meta-bind-button
label: "+1"
hidden: true
id: "spi-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: spi
    evaluate: true
    value: "x + 1"
```

```meta-bind-button
label: "-1"
hidden: true
id: "spi-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: spi
    evaluate: true
    value: "x - 1"
```

```meta-bind-button
label: "Reset"
hidden: true
id: "spi-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: spi
    evaluate: false
    value: 0
```
Current spi: `VIEW[{spi}]`

`BUTTON[spi-decrement, spi-increment, spi-reset]`

```meta-bind-button
label: "+1"
hidden: true
id: "tra-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: tra
    evaluate: true
    value: "x + 1"
```

```meta-bind-button
label: "-1"
hidden: true
id: "tra-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: tra
    evaluate: true
    value: "x - 1"
```

```meta-bind-button
label: "Reset"
hidden: true
id: "tra-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: tra
    evaluate: false
    value: 0
```
Current tra: `VIEW[{tra}]`

`BUTTON[tra-decrement, tra-increment, tra-reset]`

```meta-bind-button
label: "+1"
hidden: true
id: "men-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: men
    evaluate: true
    value: "x + 1"
```

```meta-bind-button
label: "-1"
hidden: true
id: "men-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: men
    evaluate: true
    value: "x - 1"
```

```meta-bind-button
label: "Reset"
hidden: true
id: "men-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: men
    evaluate: false
    value: 0
```
Current men: `VIEW[{men}]`

`BUTTON[men-decrement, men-increment, men-reset]`

```meta-bind-button
label: "+1"
hidden: true
id: "con-increment"
style: primary
actions:
  - type: updateMetadata
    bindTarget: con
    evaluate: true
    value: "x + 1"
```

```meta-bind-button
label: "-1"
hidden: true
id: "con-decrement"
style: default
actions:
  - type: updateMetadata
    bindTarget: con
    evaluate: true
    value: "x - 1"
```

```meta-bind-button
label: "Reset"
hidden: true
id: "con-reset"
style: destructive
actions:
  - type: updateMetadata
    bindTarget: con
    evaluate: false
    value: 0
```
Current con: `VIEW[{con}]`

`BUTTON[con-decrement, con-increment, con-reset]`
