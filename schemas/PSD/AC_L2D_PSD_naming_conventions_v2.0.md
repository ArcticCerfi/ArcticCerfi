# ArcticCerfi :: Live2D Layer Naming Conventions v2.0
(Updated October 2025) (unused)

This document explains how I name and organize layers in my PSDs for Live2D use.
It's meant to make things somewhat consistent and easy to navigate for anyone rigging my models (including myself).

If you notice anything that could make things clearer or easier, feel free to tell me! I'm always down to improve the setup.
I would highly appreciate feedback about how working with this system went for you! But it's not mandatory ofc!

And if you encounter ANY issues or questions with my PSDs, you can ALWAYS reach out to me. I'm more than happy to assist.

## CORE PRINCIPLES

- L2D-legal naming: letters, numbers, underscores, less than 64 characters
- Each layer name must be 100% unique to ensure proper automatic ID generation in L2D.
- Layers must be easily renameable by L2D's "Find & Replace" feature (e.g. renaming R to L when mirroring parts).
- Keep names as descriptive as possible but as short as context allows.
- Pass information about Layer modes, clipping, etc. onto a rigger (or myself) efficiently and hassle-free.
- Names are human-readable and follow a structure that is sufficiently verifiable by a potential future linting script.

## BASIC SCHEMA

Layer names describe structure from broad to specific.

Each Descriptor in the Hierarchy is built like this:

> ```
> [PartName]_[Attributes]
> ```

They are then chained together like this, going down the Hierarchy:

> ```
> [(OUTFIT/HAIRSTYLE/VARIANT)]__[ParentDescriptor]__[ParentDescriptor]__...__[PartDescriptor]
> ```

This does not automatically comply to the folder hierarchy inside the PSD. It's more about semantics than the actual PSD hierarchy.
Parents may be omitted in favor of name length when the part is obviously identifiable from context and it doesn't risk having duplicates.

Layer Groups are prefixed with a single underscore, so they can essentially have their own namespace and cant be duplicate with a regular layer.

## FIND & REPLACE

In order to simplify renaming parts when mirroring them in L2D, I have taken a few precautions:

Numbers cannot appear at the end of a name. If they do, an underscore is appended so L2D does not count up the digit at the end of the name when duplicating.
If there's no number at the end, it just appends "2" which can easily be batch-replaced with nothing.

L and R always need to be sorrounded by underscores. If thats not the case, replacing "_R" with "_L" can lead to words being broken (e.g. "_Reflection" -> "_Leflection").
This way you can always replace "_R_" with "_L_" and theres never an issue.

## SEPARATORS

```
"CamelCase" 	Labels					 							(e.g. "HighlightSmall")
"_" 			Joins labels & attributes on the same level 		(e.g. "Eye_L")
"__" 			Separates hierarchy levels 							(e.g. "Eye_L__Iris__HighlightSmall")
```

## VARIANTS
```
Different variations of parts (e.g. outfits & hairstyles) have an identifying prefix that is an implied parent ("__").

When multiple variants share the same part names (e.g. Bangs), the variant prefix ensures unique IDs.

The definition of what counts as part of an outfit or hairstyle is not always cut and dry.
So this rule is pretty loose and only strict for parts that risk having duplicates. (e.g. actual hair strands)
	
Outfits:
O1__  			Outfit 1
O2__  			Outfit 2
    
Hairstyles:
H1__			Hairstyle 1
H2__			Hairstyle 2
    
Generic variations:
(situational or when the base object's name cannot be modified anymore)
Alt1__			Alt-Variation 1 of the base object
```

## ATTRIBUTES

Attributes are essentially everything that describes a layer in position or function, and can be abbreviated by this standard:

### POSITIONS

```
_L / _R			Left / Right position of the part relative to the model's view
_M				Middle (Centered on the canvas)
_F / _B			Front / Back pieces of parts that are separated in depth, or a front and a general rear surface
_T / _Bo		Top / Bottom
_In / _Ou		Inner / Outer (Inner = closer to canvas center OR inner part of a geometric shape)
    
These can also be used to describe faces of geometric objects.
```

### TYPE

```
_LINE 			Layer that represents the lineart of an object. Can be a linefill that sits behind, or a lineart that sits in front.
_SHN			A shine effect that is able to be moved separately from the object. Almost always has a clipping mask and layer mode.
_REF			A reference or guide.
_DUM  			Dummy layers just for visuals in an art program, these aren't needed for rigging. Often in combination with _REF.
_DUP  			Layers that are only drawn once, but needed multiple times on the model. To be duplicated in L2D after they are rigged.
_DEL			CSP only layers. Delete before PSD export (If you see one of these in a delivery, I made a mistake. Feel free to shame me :p).
```

### MASKS & LAYER MODES

The idea here is that these can be searched up in L2D to prep the model faster. 
If these are on a group, it's meant to apply to all layers below (Atleast until offscreen drawing becomes adapted properly)

```
_CL 			Layers that are clipped onto other layers. Target should be obvious from context.
_MSK			Layers that are an invert mask for other layers.

_MUL  			Layer Mode: Multiply
_ADD  			Layer Mode: Additive
```

## EXAMPLE HIERARCHY DEMO
```

└── _Head											
│	└── _Eye_L											("Head__" was omitted here because context makes it obvious its within the head)
│		└──	_Eye_L__Iris_CL								("Eye_L__" cannot be omitted because Eye_L and Eye_R IDs would not be unique)
│		│	└── Eye_L__Iris__HighlightSmall_ADD			(-> a highlight with Layer Mode Additive -> that is part of the iris -> that is part of the left eye)
│		│	└── Eye_L__Iris__HighlightBig_ADD
│		│	└── Eye_L__Iris__Shadow_MUL
│		└──	Eye_L__Pupil
│		└──	Eye_L__Sclera
│		└── ...
└── _Body
│	└── _O1__Jacket
│		└── _O1__Jacket_O1__Arm_L						
│			└── O1__Jacket__Arm_L__Upper				(-> An upper left arm -> That is part of the Jacket -> Which is outfit number 1)
│			└── O1__Jacket__Arm_L__Lower
│			└── _O1__Jacket__Arm_L__Sleeve
│			│		└── ...
│			└── O1__Jacket__Arm_L__Lower_LINE
│			└── O1__Jacket__Arm_L__Upper_LINE
└── ...
```

## ADDITIONAL NOTES

This standard seems very strict, but in reality I use it pretty freely. When something would obviously be clearer out-of-standard, I usually put readability over consistency. For example, sometimes labels can be shortened or merged going down the hierarchy if the extra redundancy would just make it less readable than before.

Mouths are drawn as a neutral straight line to minimize rigging artifacts. The actual mouth layers are hidden by default, with a visible dummy mouth layer that can be used as a ref for the neutral mouth state.

Arm toggles are fully drawn out (with additional upper arms), but often it's more convenient to reuse the base upper arm in rigging.

Naming mistakes can obviously still happen, I'm just human and humans are sometimes dumb and miss things :)
