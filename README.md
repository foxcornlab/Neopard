# Neopard

`PRINCE`-based password candidate generator, standalone and fast.

PRINCE stands for `Probability Infinite Chained Elements` - the building
blocks of the algorithm. Think of Neopard as an advanced combinator attack:
instead of taking two wordlists and crossing them, it takes one wordlist and
builds chains of 1 to N words concatenated together.

```
wordlist: [ foo, bar ]

$ ./pp64.bin -l4 < words.txt
foo
bar
foofoo
barfoo
foobar
barbar
```

So with a 4-letter target it covers `4-letter word`, `2+2`, `1+3`, `3+1`,
`1+1+2`, `1+2+1`, `2+1+1`, `1+1+1+1` - every chain that fits. That is where
it beats plain combinators: real people stack short words, and PRINCE
models exactly that.

## `compile`

```
make            # builds pp64.bin on Linux
```

See `src/Makefile` for Windows (`pp32.exe`/`pp64.exe`) and macOS targets.

## `usage`

```
./pp64.bin [options] [<] wordlist

  --pw-min=NUM            min candidate length
  --pw-max=NUM            max candidate length
  --elem-cnt-min=NUM      min words per chain
  --elem-cnt-max=NUM      max words per chain
  --keyspace              print combination count, no output
  --wl-dist-len           output length distribution of the wordlist
  --wl-max=NUM            only load NUM words from input (0 = all)
  -s, --skip=NUM          skip NUM candidates (distributed runs)
  -l, --limit=NUM         emit at most NUM candidates
  -o, --output-file=FILE  write candidates to file
```

Pipe it straight into your cracker of choice:

```
./pp64.bin --pw-min=8 --elem-cnt-max=3 < words.txt | hashcat -m 0 hashes.txt
```

## `reading material`

- Jens Steube's talk on PRINCE from Passwords^14, Trondheim - slides: https://hashcat.net/events/p14-trondheim/prince-attack.pdf
- Matt Weir's tool deep-dive on princeprocessor: http://reusablesec.blogspot.de/2014/12/tool-deep-dive-prince.html

## `fork notes`

Neopard is a fork of hashcat's [`princeprocessor`](https://github.com/hashcat/princeprocessor)
v0.22 (MIT). Engine and algorithm are upstream; this fork carries Foxcorn Lab
branding, this README, and build fixes where needed. Upstream copyright
notices and the MIT licence are kept intact.

## `Thanks to`

- `Jens Steube`, `Steve Thomas (Sc00bz)`, `magnum` - upstream princeprocessor
- `Matt Weir` - for the analysis that explains why this tool is worth using
- The `hashcat` crew - for open-sourcing the thing in the first place

## `license`

`MIT` - see `LICENSE`. Upstream copyrights retained.

Educational and research use only. This tool generates password candidates
for auditing hashes you are authorised to test. Do not point it at systems
or accounts you do not own or have written permission to test.

> Research by `Vineeth Kumar` (`foxcornlab`). Found a bug or want a feature, open an `issue`.
