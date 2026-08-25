# IFC ReClass — Getting Started

## Why I made this

Most of the 3D building models I work with are built up in other software — Revit, ArchiCAD, and so on — before they're handed over as an IFC file (a shared, open file format that lets different building software talk to each other). Somewhere along the way, objects in that file often end up labelled as a "proxy" — a generic placeholder that just says "there's an object here" without saying what it actually is. In my experience that comes down to one of two things: either the software exporting the file was being lazy and didn't bother matching the object to its proper type, or whoever built the model didn't fully understand the IFC schema (the official rulebook for what each object type is allowed to be) and classified it wrongly by mistake. Either way, the result is the same — a placeholder object sitting in the file with no real identity. Left as proxies, those objects are invisible to anything downstream that filters or reports by object type — audits miss them, quantity counts miss them, and compliance checks miss them, because as far as the file is concerned, they're not officially anything yet.

Whatever the cause, I need to be able to get in and fix the IFC data directly — searching with proper boolean logic (combining terms with AND/OR/NOT to zero in on exactly what I'm after) and editing a whole batch of objects at once, rather than clicking through them one at a time and losing my mind. I built ReClass so I could open a file straight in the browser, find every one of these placeholder (or wrongly classified) objects, and reclassify them — in bulk — into their correct, proper object type, without needing the original modelling software at all. It reads and rewrites the raw IFC file directly, so it works entirely offline and never sends your model anywhere.

## What it actually does

- Opens an IFC file straight in your browser — nothing is uploaded anywhere, it all stays on your computer.
- Finds every "placeholder" object (technically called `IfcBuildingElementProxy`) so you can see them all in one list.
- Lets you search and filter that list to find exactly the objects you're after.
- Lets you select a batch of them and, in one go, tell the file what they actually are — e.g. "these are all walls," "these are all windows" — and (optionally) what sub-type of that thing they are (e.g. a wall could be a standard wall, a parapet, a retaining wall, and so on).
- Lets you rename them at the same time, if you want to.
- Exports a corrected copy of the file, ready to bring back into whatever viewer or software you use next.

## Basic workflow

1. **Load your file.** Click the drop-zone (or drag a file onto it) and choose your `.ifc` file.
2. **Pick a tab.** *Proxies* shows just the unclassified placeholder objects — this is usually where you'll start. *ReClassed* shows what you've already fixed in this session. *All Elements* shows everything.
3. **Search to narrow the list down.** Type a word to search — see "Searching" below for the full set of options.
4. **Tick the objects you want to fix.** Use the checkboxes, or "Select All Visible" to grab everything currently showing.
5. **Choose the real object type.** Type into the "Target IFC Class" box — you can search by the object type itself (e.g. "wall") or by a more specific term (e.g. typing "stud" will find you the object type that studs belong to, even if you can't remember its formal name).
6. **Choose the specific sub-type**, if one applies — a dropdown appears once you've picked a class, listing the official sub-types available for it.
7. **Rename them, if you'd like** — see "Renaming" below for how the naming box works.
8. **Click Apply**, then repeat for your next batch.
9. **Click Export .ifc File** when you're done, and save the corrected file.

## Searching

The search box looks for your text across three things at once: the object's name, its GlobalID (a unique reference code every object in an IFC file carries), and its Tag (an optional mark or reference code, if one's been set).

A few extra tricks:

- Type `#` followed by a number (e.g. `#4521`) to jump straight to one specific object by its position number in the file (called its Express ID) — useful if another tool has already told you exactly which object is the problem.
- Put quotes around a phrase (e.g. `"Precast Slab"`) to search for those exact words together, rather than each word separately.
- Use `(` and `)` to group parts of a search together when combining several conditions.

You can combine multiple search terms using either the full words or these shortcuts:

| Meaning | Full word | Shortcut |
|---|---|---|
| Both must be true | `AND` | `+` |
| Either can be true | `OR` | `\|` |
| Must NOT be true | `NOT` | `-` |

The shortcut needs a space on both sides to count, the same as the full word would — e.g. `Slab + - Precast` (find anything with "Slab" in it, but not "Precast"). Written together with no spaces, like `-Precast`, it's just read as ordinary text rather than an instruction.

## Renaming

The "Rename Pattern" box is optional.

- **Leave it blank** and every selected object keeps its own original name, untouched.
- **Type plain text** (e.g. `Concrete Slab`) and every selected object gets renamed to exactly that — all of them the same, replacing whatever they were called before.
- **Include `[Name]`** somewhere in what you type (e.g. `Concrete Slab — [Name]`) and each object keeps its own original name, slotted into that spot — so a slab called `Slab:200mm:123` becomes `Concrete Slab — Slab:200mm:123`, while a different one becomes `Concrete Slab — Slab:150mm:456`.

## A couple of things worth knowing

- Saving works best in Chrome or Edge, where clicking Export brings up a proper "Save As" dialog and suggests a filename based on the file you opened. Other browsers will save it automatically to your regular downloads folder instead.
- Nothing about your model ever leaves your computer — the whole tool runs in the browser tab itself, with no server involved.
