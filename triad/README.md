# Triad

Three SNES games in one cartridge image: **Mega Man X** (USA, Rev 1), **Mega Man X2** (USA) and
**Mega Man X3** (USA), combined into a single 12 MB (96 Mbit) image for the Cx4 **SO96** board.
A proof of concept.

- **Each game runs as itself.** Its code runs from a new place in the image, with every address
  it uses moved to match, and plays like the original cartridge. Graphics and sounds the games
  share are stored once.
- **A debug menu in every game:** hold **L + R + Select**. Up/Down move, Left/Right change a
  value, A acts, Start closes and the game resumes.
  - Start any stage.
  - Turn each armour piece on or off.
  - Per stage: heart tank, sub-tank, beaten, weapon.
  - Extras per game: X's Hadouken; X2's Shoryuken, X-Hunter placement and Zero's parts;
    X3's enhancement chips, ride-armour chips, gold armour and Z-Saber.
  - A sound-effect test.
  - The top row switches to another of the three games, or restarts this one.

## What runs it

The SO96 board is new, so stock emulators and flashcart firmware do not know it.

| where | what you need |
|---|---|
| PC / RetroArch | the bsnes fork, branch `blancain-runtime`: https://github.com/blancain/bsnes |
| SD2SNES (mk2) | the sd2snes fork's firmware and Cx4 core, branch `blancain-runtime`: https://github.com/blancain/sd2snes |

## Making the ROM

You need your own dumps of the three cartridges. No game data is distributed here.

| game | SHA-1 |
|---|---|
| Mega Man X (USA) (Rev 1) | `c65216760ba99178100a10d98457cf11496c2097` |
| Mega Man X2 (USA) | `637079014421563283cded6aeaa0604597b2e33c` |
| Mega Man X3 (USA) | `b226f7ec59283b05c1e276e2f433893f45027cac` |

**In the browser:** open https://blancain.github.io/lab/triad/ and drop the three files onto it.
Everything runs on your machine; nothing is uploaded. The page:

- names each dump by its SHA-1 and removes a copier header if one is present;
- joins them and applies the patch;
- checks the patch's CRC32s and the result's SHA-1 before offering `triad.sfc`.

**With a patcher (Flips, beat, RetroArch soft-patching):** `triad.bps` applies to the three
files joined end to end in the order above, with headers removed:

```sh
cat "Mega Man X (USA) (Rev 1).sfc" "Mega Man X2 (USA).sfc" "Mega Man X3 (USA).sfc" > triad-source.sfc
flips --apply triad.bps triad-source.sfc triad.sfc
```

On Windows: `copy /b x1.sfc + x2.sfc + x3.sfc triad-source.sfc`.

`SHA256SUMS` lists the files in this folder.

## Licence

MIT (see `../LICENSE`) for the page and for the code and data the patch adds. Mega Man X, X2
and X3 are Capcom's. This project is not affiliated with or endorsed by Capcom, and the patch
contains none of the games' bytes.
