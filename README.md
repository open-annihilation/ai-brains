# OA Brains

Brains are Lua packages that play a computer seat in
[Open Annihilation](https://github.com/open-annihilation/open-annihilation).
A brain reads what its seat may know about the match, gives its units orders,
and can say things in the chat. The engine runs it in a sandbox with a fixed
instruction allowance a tick, so a brain plays the same way on every computer;
a brain that runs past its allowance or its time is stopped, and the seat goes
back to the built-in computer player.

This repository holds the brains the Open Annihilation project maintains. Each
folder at the top is one brain package, ready to pack into a `.oabrain` file.

| Brain | What it is |
| --- | --- |
| [`classic`](classic/) | The engine's own computer player, written in Lua. It plays exactly as the built-in computer player does, and is the reference for how a brain is put together. |

## Using a brain

Brains need a version of Open Annihilation with brain support (Classic asks
for 0.8.6 or later). Install one from the Library's **Brains** tab, or open a
`.oabrain` file with the game, then choose it for a computer seat in the
skirmish setup or the battle room. A seat with no brain plays the built-in
computer player, as it always has.

## Packing and checking

`oa-tool` comes with the game. Pack a folder into a `.oabrain`, then check it
against your game data before you share it:

```sh
oa-tool brain pack classic --out classic.oabrain
oa-tool brain check classic.oabrain --game-dir /path/to/totala
```

`brain check` loads the brain, runs its `fits` and `start` against the game's
unit definitions, and reports the instructions and memory each phase used.
Add `--mod FOLDER` to check it against a mod, and `--level LEVEL` for one of
its levels. A finished check ends with `result: ok`.

## Writing a brain

A brain folder holds `oabrain.yaml` (the manifest: id, name, levels, what it
works with), an entry script (usually `brain.lua`) and any modules it loads
from `lua/`. A brain that uses neural networks also carries them as `.oann`
files under `nets/`. The engine documents the format and the whole Lua API:

- [The OABrain standard](https://github.com/open-annihilation/open-annihilation/blob/lua-brains/docs/brains/oabrain-standard.md):
  the package, the manifest, the runtime, its budgets, and every call a brain
  may make.
- [Writing a brain](https://github.com/open-annihilation/open-annihilation/blob/lua-brains/docs/brains/authoring.md):
  a first brain from an empty folder to a played match, then a walk through
  Classic.

To add a brain here, give it a folder of its own named after its manifest
`id`, with its own `README.md` saying what it does and how it plays, and its
licence. Pack and check it as above, and play it in at least one headless
match against the built-in player before you open a pull request.

## Classic stays in step with the engine

The engine ships Classic packed beside the game and recognises it by the
exact bytes of the files in its folder. Only a byte-for-byte copy of the
engine's `brains/classic` is treated as Classic, so this folder is that copy
and changes together with the engine's: a change to Classic goes to both, in
the same form. That is also why its README names files in the engine
repository, where the built-in player and Classic's checks live.
`.gitattributes` keeps every file here exactly as committed, so a checkout on
any system packs the same bytes.

## Licence

Copyright The Open Annihilation Authors (see [COPYRIGHT](COPYRIGHT)). The
brains are distributed under the GNU General Public License, version 3 only
([LICENSE](LICENSE)), unless a brain's own folder states other terms. Each
brain folder carries its licence, so a packed `.oabrain` carries it too.
