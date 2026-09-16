<!-- SPDX-License-Identifier: GPL-2.0-only -->
# Research notes: Data-type profiling (Cauldron 2026)

Companion to `index.html`. Facts checked Sep 2026; re-verify before
presenting. Describe paper ideas in own words only (licensing).

## Related work (talk: "Related work" slide)

- DProf (EuroSys'10, Pesterev/Zeldovich/Morris): IBS + debug regs to
  kernel data types. The direct ancestor.
  https://pdos.csail.mit.edu/papers/dprof-eurosys10.pdf
- SHERIFF (OOPSLA'11) / PREDATOR (PPoPP'14), Liu & Berger: precise via
  process isolation/instrumentation, heavy. We sample: cheap,
  production-safe, statistical.
  https://people.cs.umass.edu/~emery/pubs/fs-oopsla2011.pdf
- Intel VTune false-sharing recipe / Memory Access analysis; AMD uProf
  Cache Analysis (IBS-OP hot/false-sharing lines).
  https://www.intel.com/content/www/us/en/docs/vtune-profiler/cookbook/2025-4/false-sharing.html
- TypeCraft (OSDI'26, Li/Xu Liu/Namhyung Kim/Jones/Alexandrov/Li):
  perf-integrated type profiler, per-instruction type+field costs,
  field-reorder wins. Complementary: instruction costs there, export +
  pahole action here. Namhyung bridges both. Public abstract only.
  https://www.usenix.org/conference/osdi26/presentation/li-zecheng
  Slides: https://www.usenix.org/system/files/osdi26_slides-li_zecheng.pdf
- Kernel doc officially pairs `perf c2c` + pahole for false sharing;
  we add the intra-type half c2c cannot see.
  https://docs.kernel.org/kernel-hacking/false-sharing.html
- perf c2c origins (2016): Joe Mario prototype, Jiri Olsa upstream.
  https://joemario.github.io/blog/2016/09/01/c2c-blog/
- Namhyung's lineage decks: LPC2023 data-type goal/method, LPC2024
  type info in perf (v6.8/v6.10/v6.12).
  https://lpc.events/event/17/contributions/1616/
  https://lpc.events/event/18/contributions/1833/

## Compiler features (talk: "What compilers give us" slide)

- `-Wpadded` (gcc, clang, off by default); clang also
  `-Wpadded-bitfield`.
- Layout dumps: clang `-Xclang -fdump-record-layouts`, GCC
  `-fdump-lang-class` (C++ only). pahole works without source.
- No PGO struct field reorder in GCC (`-fipa-struct-reorg` removed in
  4.8) or LLVM. `-freorder-*` is code-only.
- BOLT / AutoFDO / Propeller: code layout only. Data is the gap.
- `preserve_access_index` (both compilers): CO-RE relocations.

## Traps: do NOT say on stage

- There is no Google PLDI 2011 struct paper. Cite Hundt CGO 2006
  (layout + advisory mode) and Raman/Hundt/Mannarswamy CGO 2007
  (multithreaded + false sharing).
- MemProf is Lachaize et al. (USENIX ATC'12, NUMA), not Berger.
- TypeCraft: own words only, never quote the paper.
