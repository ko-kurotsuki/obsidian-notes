---
des: 1
pro: 1
spi: 2
tra: 2
men: 0
con: 0
---

Warlock
Casting trait: Charge caster
A warlock cannot cast spells unless they have gathered charges to do so through specific actions of devotion which are listed under their selected patron.
Minor charges cover spell from capacity 1 to 4, significant cover from 5 to 8, and capacity 9 and 10 spells require a major charge. All warlock spells are cast instantly.
Personal Trait: Patron
A warlock must select a patron. They gain the warlock bonus for that patron.
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
