# This program has been superseded
A year after having made this, I made a [twofold reducer](https://github.com/DroneBetter/ChessPieceVision/blob/main/twofold_reducer.py), with bijective indexing methods for emulating lists of integers nonequivalent under action of bitwise reversal without storing them (similarly to for square binary matrices with the [eightfold reducer](https://github.com/DroneBetter/ChessPieceVision/blob/main/eightfold_reducer.py)), the `__main__` in there contains a bitwise reimplementation of this program that is 6 times faster (and a much nicer program), use that instead.

See the original description below

# WolframTapeExhauster
A Python program that finds oscillators in Stephen Wolfram's elementary cellular automata within all possible tapes (both cylindrical and bounded) of given widths and graphs periods with respect to width.

### Why?

Recently, when reading the ConwayLife forums, I came upon a thread regarding the [isotropic non-totalistic](https://conwaylife.com/wiki/Isotropic_non-totalistic_cellular_automaton) rule [b34kz5e7c8s23-a4ityz5k](https://conwaylife.com/forums/viewtopic.php?f=11&t=4330), which is eleven transitions (edges across the 102-dimensional hypercube of the INT rulespace) from b3s23, Conway's game of life (of which there are choose(102,11)=178152655364880 such rules). As you may expect, it behaves reasonably similarly on the scale of individual cells, and shares many small still lifes and oscillators, even two spaceships (the glider and lightweight spaceship, though the latter of which deviates every second iteration) but the emerging larger-scale self-sustaining/propagating patterns behave differently, with a greater range of common naturally-occurring elementary spaceships, the ["stag" oscillator](https://catagolue.hatsya.com/object/xp3_3668663/b34kz5e7c8s23-a4ityz5k) (known from another rule one transition away from it, called mooselife), and the [blinker-puffer](https://catagolue.hatsya.com/object/yl18_1_3_87d145c476d14227bde70119797af30c/b34kz5e7c8s23-a4ityz5k) (that leaves a trail of blinkers behind it and occurs in ~1 in 10 16\*16 "soups" of random noise). The souprate seemed low, which I assumed was because people hadn't searched it very much, meaning interesting things remained to be seen, but it was actually because chaos stabilises more slowly in it (so soups takes longer to simulate to periodicity/classification than the others against which you judge soupcount). Regardless, I found some interesting things in asymmetrical soups (like a [dividing reaction stabilised by a period-8 spaceship to be a period-16 tagalong](https://catagolue.hatsya.com/object/xq16_e9bgzz79d6/b34kz5e7c8s23-a4ityz5k) and a [rake for spaceships that move at 10/26ths of the speed of light](https://catagolue.hatsya.com/object/yl1872_2_1171_9131d187da1d805154cb67f72d6241e9/b34kz5e7c8s23-a4ityz5k) and some [reactions between blinker-puffers that produce more blinker-puffers perpendicularly](https://catagolue.hatsya.com/object/zz_QUADRATIC/b34kz5e7c8s23-a4ityz5k) (growing quadratically)), then looked in the forum thread again and saw some high-period oscillators found in censuses of symmetrical soups, and decided to search them myself.

During my search, I encountered [this period-4 oscillator](https://catagolue.hatsya.com/object/xp4_g8gidrk4o0o4krdig8gz1169ba5210125ab9611zo8mpdlq4o0o4qldpm8oz0104bd2210122db401/b34kz5e7c8s23-a4ityz5k), which is an instance of [a known phenomenon](https://conwaylife.com/forums/viewtopic.php?f=11&t=4330#p90947) in the rule, where two arbitrarily long barges allow patterns of symmetrical pairs of cells in between them dance back and forth at the speed of light. This radial oscillator contained four of these. The simplest example is the [period-4 one](https://catagolue.hatsya.com/object/xp4_8ka56o8gzx2521/b34kz5e7c8s23-a4ityz5k), which had occurred 20 times in 5 such symmetrical soups already. However, this one differed in that while one end was bordered by an off cell like usual, the other was fixed on. After stabilising a a monomerisation with the always-on end, I found they simulate in rule 150 (where cells change state only if their neighbours are different (or, put another way, are their neighbourhood's sum modulo 2)), so if I could find a period-13 oscillator in rule 150, I would have found one in b34kz5e7c8s23-a4ityz5k also (13 being the lowest period with no explicit examples), so created this program. However, they don't exist in tapes of width <=25, but the existence of other high prime-numbered periods so far implies that they do in some width, though probabilistic searches haven't found one either.
