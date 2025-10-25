# ArcticCerfi :: Live2D Layer Naming Conventions v2.0 
(Updated October 2025) (unused)

This document explains how I name and organize layers in my PSDs for Live2D use.
It's meant to make things consistent, clear, and easy to navigate for anyone rigging my models (including myself).
It might look detailed, but it's mostly straightforward once you see it in action, it's just just a way to avoid confusion and keep everything tidy and predictable.

If you notice anything that could make things clearer or easier, feel free to tell me! I'm always down to improve the setup.
I would highly appreciate feedback about how working with this system went for you! But it's not mandatory ofc!

## CORE PRINCIPLES

- L2D-legal characters: letters, numbers, underscores.
- Each layer name must be 100% unique to ensure proper automatic ID generation in L2D.
- Layers must be easily renameable by L2D's "Find & Replace" feature (e.g. renaming R to L when mirroring parts).
- Keep names as descriptive as possible but as short as context allows.
- Pass information about Layer modes, clipping, etc. onto a rigger (or myself) efficiently and hassle-free.
- Names are human-readable and follow a structure that is sufficiently verifiable by a script.


## BASIC SCHEMA

Layer names describe structure from broad to specific.
Each name can be read like a path: e.g. Part -> Subpart -> Attributes


Each Descriptor in the Hierarchy is built like this:

> ```
> [PartName]_[Attributes]
> ```

They are then chained together like this, going down the Hierarchy:

> ```
> [ParentDescriptor]__[ParentDescriptor]__...__[PartDescriptor]
> ```


Parents may be omitted in favor of name length when the part is obviously identifiable from context and it doesn't risk having duplicates.

Layer Groups are prefixed with a single underscore.

Numbers cannot appear at the end of a name. If they do, an underscore is appended.

## SEPARATORS

```
"CamelCase" 	Words that belong together 							(e.g. "HighlightSmall")
"_" 			Joins labels & attributes on the same level 		(e.g. "Eye_L")
"__" 			Separates hierarchy levels 							(e.g. "Eye_L__Iris__HighlightSmall")
```

## VARIANTS
```
Different Variations of parts (e.g. outfits & hairstyles) have an identifying prefix that is presented as an implied parent ("__").
	
Outfits:
O1__  			Outfit 1
O2__  			Outfit 2
    
Hairstyles:
H1__			Hairstyle 1
H2__			Hairstyle 2
    
Situational variations (unplanned or one-off):
Alt1__			Alt-Variation 1 of the base object
```

## ATTRIBUTES

### POSITIONS

```
_L / _R			Left / Right position of the part relative to the model's view
_F / _B			Front / Back pieces of parts that are separated in depth or a front and a general rear surface
_T / _Bo		Top / Bottom
_In / _Ou		Inner / Outer (Inner = closer to canvas center OR inner part of a shape)
    
These can also be used to describe faces of geometric objects.
L & R are always encased in underscores to ensure easy Find & Replacing in L2D.
```

### TYPE

```
_LINE 			Layer that prepresents the lineart of an object. Can be 
_REF			A reference or guide.
_DUM  			Dummy layers just for visuals in an art program, these aren't needed for rigging. Often in combination with _REF.
_DUP  			Layers that are only drawn once, but needed multiple times on the model. To be duplicated in L2D after they are rigged.
_DEL			CSP only layers. Delete before PSD export (If you see one of these in a delivery, I made a mistake. Feel free to shame me :p).
```

### EFFECTS AND MASKS

```
_CL 			Layers that are clipped onto other layers. Target should be obvious from context. If applied to a group its meant to use offscreen rendering OR apply to all child layers, in case of an older Cubism version.
_MSK			Layers that are an invert mask for other layers.
_SHN			A shine effect that is able to be moved separately from the object. Almost always has a clipping mask and layer mode.
```

### LAYER MODES

```
_MUL  			Layer Mode: Multiply
_ADD  			Layer Mode: Additive

Layers without one of these are implied to use "Normal".
```

## EXAMPLE HIERARCHY DEMO
```
_ROOT
└── _Head											
│	└── _Eye_L											("Head__" was omitted here because context makes it obvious its within the head)
│		└──	_Eye_L__Iris_CLIP							("Eye_L__" cannot be omitted because Eye_L and Eye_R IDs would not be unique)
│		│	└── Eye_L__Iris__HighlightSmall_ADD			(-> a highlight with Layer Mode Additive -> that is part of the iris -> that is part of the left eye)
│		│	└── Eye_L__Iris__HighlightBig_ADD
│		│	└── Eye_L__Iris__Shadow_MUL
│		└──	Eye_L__Pupil
│		└──	Eye_L__Sclera
│		└── ...
└── _Body
│	└── _Jacket_O1
│		└── _Jacket_O1__Arm_L
│			└── Jacket_O1__Arm_L__Upper
│			└── Jacket_O1__Arm_L__Lower
│			└── _Jacket_O1__Arm_L__Sleeve
│			│		└── ...
│			└── Jacket_O1__Arm_L__Lower_LINE
│			└── Jacket_O1__Arm_L__Upper_LINE
└── ...
```

## OTHER NOTES

IMPORTANT: Names in this system can turn out quite long at high separation detail. I always make sure they don't exceed L2Ds character cap. But if you glue 2 long-named layer together and your glue's ID turns out too long, your exports will fail and L2D will not give you a warning besides an error in the console. So be mindful of that and rename your glues if they turn out too long!

Mouths are drawn as a neutral straight line to minimize rigging artifacts. The actual mouth layers are hidden by default, with a visible Mouth_DUMMY layer that should be deleted or hidden before rigging.

Arm toggles are fully drawn out (with additional upper arms), but often it's more convenient to reuse the base upper arm in rigging.

Naming mistakes can still happen, I'm just human and humans are sometimes dumb and miss things :)
