# SQLite in spaced C

This fork is the SQLite source tree with every `.c` and `.h` file laid out
as **spaced C** by [Offside](https://offside.grok.me/), the indentation layout
pass (not a compiler, and not a rewrite of the engine).

Upstream commit ported: `3ee9f40` (`master` of [sqlite/sqlite](https://github.com/sqlite/sqlite)).

## What changed

- A header colon plus a deeper indent stands in for `{` … `}`.
- A finished line is the statement. Semicolons that only terminated a statement are gone.
- `struct`, `union`, and `enum` still close on the dedent. A declarator name that followed `}` sits on that dedent line (`mem5` under `struct Mem5Global:`).
- Initializer and enum braces stay braces. Ternaries, labels, `asm`, `_Generic`, and attributes stay as they were.
- Preprocessor lines pass through, including `#if` / `#endif` nests inside functions.

359 C sources, about 15 MB. No logic was edited by hand. Java, JavaScript, Tcl, Lemon grammars, and scripts were left alone.

## Reading it

```c
static void memsys5Unlink(int i, int iLogsize):
    int next, prev
    next = MEM5LINK(i)->next
    prev = MEM5LINK(i)->prev
    if( prev<0 ):
        mem5.aiFreelist[iLogsize] = next
    else:
        MEM5LINK(prev)->next = next
```

## Building

A C compiler will not accept this layout. Run the files back through Offside
(**spaced to host**, language C) before `configure` / `make`. That reverse
pass is lossy on a few SQLite idioms (a one-line `if` grows braces; some
`else` closings shift). The spaced text in this tree is the port; do not treat
a round trip as a byte-identical copy of upstream.
