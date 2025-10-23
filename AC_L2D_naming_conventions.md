# ArcticCerfi :: Live2D Layer Naming Conventions v2.0 (Updated October 2025)

This document explains how I name and organize layers in my PSDs for Live2D use.
It's meant to make things consistent, clear, and easy to navigate for anyone rigging the model (including myself).
It might look detailed, but it's mostly straightforward once you see it in action, it's just just a way to avoid confusion and keep everything tidy and predictable.

If you notice anything that could make things clearer or easier, feel free to tell me! I'm always down to improve the setup.

## CORE PRINCIPLES

- L2D-legal characters: letters, numbers, underscores.
- Each layer name must be 100% unique to ensure proper automatic ID generation in L2D.
- Layers must be easily renameable by L2D's "Find & Replace" feature (e.g. renaming _R_ to _L_ when mirroring parts).
- Keep names as descriptive as possible but as short as context allows.
- Pass information about Layer modes, clipping, etc. onto a rigger (or myself) efficiently and hassle-free.
- Consistency beats brevity: only shorten when scope is obvious.
- Names are human-readable and follow a structure that is sufficiently verifiable by a machine.


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


## SEPARATORS

```
"CamelCase" 	Words that belong together 							(e.g. "HighlightSmall")
"_" 			Joins labels & attributes on the same level 		(e.g. "Eye_L")
"__" 			Separates hierarchy levels 							(e.g. "Eye_L__Iris__HighlightSmall")
```


## ATTRIBUTES

### VARIATIONS
```
Different Variations of parts (e.g. for outfits, hairstyles, etc.) need an identifying attribute.
The attribute only needs to be present in the chain once.

Outfits:
_OF1  			Outfit #1
_OF2  			Outfit #2

Arm toggles:
_ATogMic		Holding a microphone
_ATogPet		Holding a mascot/pet

Hairstyles:
_HS1			Hairstyle #1
_HS2			Hairstyle #2

...

Situational variations (unplanned or one-off):
_ALT1			Variation 1 of the base object
_ALT2			Variation 2 of the base object

...
```
### POSITIONS

```
_L / _R			Left / Right position of the part relative to the model's viewpoint, or if nested, the parent part
_F / _B			Front / Back pieces of parts that are separated in depth (Typically parts that go around something else)
_Top / _Bot		Top / Bottom
```

### TYPE

```
_LINE 			A solid linefill layer that sits behind a color layer, often used when 2 pieces share the same lineart.
_LINEART		A "hollow" lineart layer that sits above a color layer. More rare, thus the longer naming.
_REF			A reference or guide.
_DUMMY			Layers just for visuals in an art program, these aren't needed for rigging. Often in combination with _REF.
_DUP  			Layers that are only drawn once, but needed multiple times on the model. To be duplicated in L2D after they are rigged.
_DELETE			CSP only layers. Delete before PSD export (If you see one of these in a delivery, I made a mistake. Feel free to shame me :p).
```

### EFFECTS AND MASKS

```
_MASK 			Layers that are clipped onto other layers. Target should be obvious from context.
_INVMASK		Layers that are an invert mask for other layers.
_SHINE			A shine effect that is able to be moved separately from the object. Almost always has a clipping mask and layer mode.
```

### LAYER MODES

```
_MUL  			Layer Mode: Multiply
_ADD  			Layer Mode: Additive

Layers without one of these are implied to use "Normal".
```

### FACES & GEOMETRY

```
"_Face" describes visible planes of geometric objects. The definition of what counts as a face, is very loose here.

Often:
_Face_F			Front face of an object (most visible to viewer)
_Face_B 		Back face or a general rear surface that isn't the front

More rarely:
_Face_L			Left side face of an object
_Face_R 		Right side face of an object
_Face_T 		Top face of an object
_Face_Bo 		Bottom face of an object
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
│	└── _Jacket_OF1
│		└── _Jacket_OF1__Arm_L
│			└── Jacket_OF1__Arm_L__Upper
│			└── Jacket_OF1__Arm_L__Lower
│			└── _Jacket_OF1__Arm_L__Sleeve
│			│		└── ...
│			└── Jacket_OF1__Arm_L__Lower_LINE
│			└── Jacket_OF1__Arm_L__Upper_LINE
└── ...
```
