# Amalur
Research project on the game Kingdoms of Amalur: Reckoning


## Focus
The main focus currently lies on reversing the Havok Script vm.

Havok Script is a rebranded version of Kore VM which Havok acquired in 2010
>Kore VM was developed by the Irish company New Game Technologies and is a highly optimized implementation of the Lua scripting language

[source](https://www.havok.com/havok-announces-the-acquisition-of-kore-vm-product/)

Current status is in [korevm.md](./korevm.md) and [functions.txt](./functions.txt)

### Roadmap

After that I'd like to focus on readin/extracting the data files used by the engine.

Eventually it may be possible to create a patch to fix some of the most game-breaking bugs.

### Links

- BIG file extractor: http://raptor-cestiny.cz/download/kingdoms-of-amalur-reckoning-big-files-extractor.html
- LUA C interface https://www.lua.org/pil/24.html
- LUA 5.1 vm introduction http://luaforge.net/docman/83/98/ANoFrillsIntroToLua51VMInstructions.pdf
