# `<InputsWinfoBlock>`

* [Introduction](#introducation)
* [Slots](#slots)
  * [`info` slot](#info-slot)
  * [`input` slot](#inputs-slot)
* [Attributes](#attributes)
  * [`id`](#id)
  * [`heading`](#heading)
  * [`h-level`](#h-level)
  * [`alternate`](#alternate)
  * [`swap`](#swap)
  * [`input-percent`](#input-percent)
* [Alternating sides for inputs](#alternating-sides-for-inputs)
  * [`inputs-w-info--alternate`](#inputs-w-info--alternate)
  * [`inputs-w-info--alternate--rev`](#inputs-w-info--alternate--rev)
  * [Overriding altarnation with `swap`](#overriding-altarnation-with-swap)


## Introducation

The `<InputsWinfoBlock>` component is designed to be a very simple,
zero state component for visual layout purposes only. It is intended
for rendering group of user input fields, along with some information
about the user inputs and a title for the input group.

It is intended to replace a fieldset/legend + inputs group.

It does not have any interactive state. Instead, delegating all 
interactive state to it's slotted components' content.

---

## Slots

`<InputsWinfoBlock>` delegates all of the heavy lifting of managing 
content and state to it's child (slotted) content.

It has two slots. One for rendering input fields and another for rendering descriptive info about the input fields as a group.

### `info` slot

The info slot is intended for rendering descriptive information about 
the group of input fields being rendered in the [`inputs` slot](#inputs-slot)

For accessibility reasons, the info slot's component should be wrapped in an element with an `id` attribute that matches the `id` used in the `aria-describedby` attribute of the element with the `role="group"` in the [`inputs` slot](#inputs-slot).

By default, the `info` slot is rendered first. But this can be changed by using the [`alternate`](#alternate) and/or the [`swap`](#swap) attributes. 

Also, by default, on larger screens, the `info` slot occupies 50% of the available horizontal space but is subject to the width of the `inputs` slot's width if [`input-percent`](#input-percent) is set to something.other than `50`

### `inputs` slot

---
## Attributes

### `id`

|  Required  |    Type    |   Default:   | Variable name |
|------------|------------|--------------|---------------|
| _required_ | _{string}_ | _no default_ |  `props.id`   |

The ID used for deep linking into the page.

The same ID should also be used as the prefix for IDs of child content.

### `heading`

|  Required  |    Type    |   Default:   |  Variable name  |
|------------|------------|--------------|-----------------|
| _required_ | _{string}_ | _no default_ | `props.heading` |

The main heading of the block (the value used in the `legend` element 
if this component used a  fieldset as its wrapper)

### `h-level`

|  Required  |    Type    | Default | Min | Max | Variable name  |
|------------|------------|---------|-----|-----|----------------|
| _optional_ | _{number}_ |   `2`   | `1` | `6` | `props.hLevel` |

By default this component renders the heading as an `h2` but can be 
forced to render as any level heading appropriate to it's context.

### `input-percent`

|  Required  |    Type    | Default | Min  | Max  |    Variable name     |
|------------|------------|---------|------|------|----------------------|
| _optional_ | _{number}_ |   `2`   | `10` | `90` | `props.inputPercent` |

`input-percent` represents the percentage width of of the 
[`inputs` slot](#inputs-slot) and thus indirectly influences the 
width of the [`info` slot](#info-slot).

### `swap`

|  Required  |    Type     | Default | Variable name |
|------------|-------------|---------|---------------|
| _optional_ | _{boolean}_ | `FALSE` | `props.swap`  |

> __Note:__ The attributes `left`, `right` & `swap` are mutually 
>           exclusive. 
>           If two or more are present, the first one (in the order 
>           listed here) wins

If `swap` is present the visual order of [`info` slot](#info-slot) 
and [`inputs` slot](#inputs-slot) are swapped in larger screens. 
This is useful if you're wrapping a list of `<InputsWinfoBlock>` 
in `.inputs-w-info--alternate` or `.inputs-w-info--alternate--rev` 
because it responds to its relative position.

### `left`

|  Required  |    Type     | Default | Variable name |
|------------|-------------|---------|---------------|
| _optional_ | _{boolean}_ | `FALSE` | `props.left`  |

> __Note:__ The attributes `left`, `right` & `swap` are mutually 
>           exclusive. 
>           If two or more are present, the first one (in the order 
>           listed here) wins

If `left` is present the [`inputs` slot](#inputs-slot) is forced to 
the left of the block (regardless of alternate or swap styling).

### `right`

|  Required  |    Type     | Default | Variable name |
|------------|-------------|---------|---------------|
| _optional_ | _{boolean}_ | `FALSE` | `props.right` |

> __Note:__ The attributes `left`, `right` & `swap` are mutually 
>           exclusive. 
>           If two or more are present, the first one (in the order 
>           listed here) wins

If `right` is present the [`inputs` slot](#inputs-slot) is forced to 
the right of the block (regardless of alternate or swap styling).

> __Note also:__ By default the [`inputs` slot](#inputs-slot) is on 
>                the right. Unless you're using 
>               [`inputs-w-info--alternate` or `inputs-w-info--alternate--rev`](#alternating-sides-for-inputs), 
>               it makes no sense to include the `right` attribute.

---

## Alternating sides for inputs

For aesthetic reasons it may be desirable to alternate which side the inputs sit on on larger screens. To do this, we have two classes that can be applied to the direct parent of `<InputsWinfoBlock>`.

* `inputs-w-info--alternate`
* `inputs-w-info--alternate--rev`

### `inputs-w-info--alternate`

If the class `inputs-w-info--alternate` is applied to the direct 
parent of `<InputsWinfoBlock>` then, every odd numbered 
`<InputsWinfoBlock>` block's [`info` slot](#info-slot) and 
[`input` slot](#input-slot) are swapt when rendered on lager screens.

### `inputs-w-info--alternate--rev`

If instead `inputs-w-info--alternate--rev` applied to the direct 
parent of `<InputsWinfoBlock>` then, every even numbered 
`<InputsWinfoBlock>` block's [`info` slot](#info-slot) and 
[`input` slot](#input-slot) are swapt when rendered on lager screens.

If both `inputs-w-info--alternate` *and* `inputs-w-info--alternate--rev` 
are present, `inputs-w-info--alternate` wins. 
(i.e. Every second `<InputsWinfoBlock>` block's inputs are on the left)


### Overriding altarnation with `swap`

If for some reason, you want to one `<InputsWinfoBlock>` to have the
same visual order in larger screens as it's preceeding (and following)
block, you can also include the [`swap`](#swap) attribute on that
`<InputsWinfoBlock>` block.


---

See info about [Vite and VueJS + Typescript](README.vite.md)

