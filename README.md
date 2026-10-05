# Detyra 1: Teknikat e painformuara të kërkimit

Arbër Hoti · Inteligjenca Artificiale 2026/27

Ekzekutimi (Python 3.10+, pa librari të jashtme):

```
python golfers.py 20        # Social Golfers, DFS me backtracking, kufiri i kohës 20 s
python sudoku.py            # Sudoku, BFS me backtracking, nivelet easy / medium / hard
python sudoku.py hard       # vetëm një nivel
python latin.py 5           # Latin Square, IDDFS me backtracking
```

Çdo program e kontrollon vetë zgjidhjen (funksioni `check` / `objective` duhet të japë 0 shkelje).

---

## 1. Social Golfers Problem

**Modelimi.** Kemi n = g × p = 8 × 4 = 32 lojtarë. Çdo javë çdo lojtar luan saktësisht një herë, në saktësisht një grup me 4 lojtarë. Dy lojtarë nuk guxojnë të jenë në të njëjtin grup më shumë se një herë gjatë gjithë javëve. Pyetja: sa javë w mund të planifikohen? Çdo lojtar takon 3 lojtarë në javë dhe ka 31 lojtarë të tjerë, prandaj w ≤ ⌊31/3⌋ = 10.

**Reprezentimi.** Variablat: `schedule[w][g][k]`, lojtari në vendin k (0..3) të grupit g (0..7) në javën w. Domeni: {0, …, 31}. Kufizimet (hard):
1. në çdo javë secili lojtar paraqitet saktësisht një herë;
2. çdo çift lojtarësh takohet më së shumti një herë (matrica `met[i][j]`).

Thyerja e simetrisë: brenda grupit lojtarët janë në rend rritës, dhe çdo grup i ri fillon me lojtarin më të vogël të lirë (grupet dhe renditja brenda grupit nuk kanë rëndësi).

**Objective function.** f(S) = Σ_{i<j} max(0, takime(i, j) − 1), numri i takimeve të përsëritura. Zgjidhje e vlefshme ka f = 0; qëllimi është w maksimal me f = 0.

**Landscape.** Një javë = ndarje e 32 lojtarëve në 8 grupe të paetiketuara me nga 4:
32! / ((4!)^8 · 8!) = 59 287 247 761 257 140 625 ≈ 5.93 · 10^19.
Për w javë: (5.93 · 10^19)^w, p.sh. për 10 javë ≈ 5.4 · 10^197.

**Algoritmi.** `golfers.py`: DFS rekursiv. Java plotësohet slot pas sloti; lojtari vendoset vetëm nëse nuk është takuar me asnjë anëtar të grupit (kontrolli para zbritjes në pemë), përndryshe bëhet backtracking (hiqet nga grupi dhe zhbëhen takimet).

**Rezultati.** Brenda 20 sekondave DFS planifikon **5 javë** (183 nyje), të verifikuara nga `check()`. Java e 6-të nuk gjendet me këtë qasje javë-pas-jave, sepse javët e mëparshme nuk rishikohen. Kufiri teorik është 10 javë.

---

