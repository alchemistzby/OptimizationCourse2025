# OptimizationCourse2025

This repository contains my submission for the final project of the Optimization Methods in Machine Learning course at Peking University.

## The original reference

```bib
@article{frohner2023approaching,
  title={Approaching the traveling tournament problem with randomized beam search},
  author={Frohner, Nikolaus and Neumann, Bernhard and Pace, Giulio and Raidl, G{\"u}nther R},
  journal={Evol. Comput.},
  volume={31},
  pages={233--257},
  year={2023},
  publisher={MIT Press One Rogers Street, Cambridge, MA 02142-1209, USA journals-info~…}
}
```

## Installation

Installation of dependencies assuming a python3 with ortools installed in the path:

```linux
conda create -n ttp python=3.12

conda activate ttp

pip install ortools==9.9.3963

conda install -c conda-forge julia=1.10.2

julia --project=. -e "import Pkg; Pkg.instantiate(verbose=true)"

PYTHON=$(which python3) julia --project=. -e 'using Pkg; Pkg.build("PyCall")'

pip install matplotlib
```

## Randomized beam search

To precalculate the lower bounds for teams' states of an instance, to be saved into a pickled and bz2 compressed numpy array:

> julia --project=. ttp_bounds_precalculation.jl insts/circ/circ4.txt 3 data/circ4_cvrph.pkl.bz2 true

To subsequently call the randomized beam search approach with shuffled team ordering and relative noise of 0.001:

> julia --project=. ttp_beam_search.jl insts/circ/circ4.txt 3 true data/circ4_cvrph.pkl.bz2 10000 true random none 0.001 -1 false

For the latter, there is also an iterative variant, which increases the beam width by a factor every number of runs until either a time or maximum beam width is hit:

> julia --project=. ttp_beam_search_ortools_iter.jl insts/NL/nl10.txt 3 true 3600 128 32768 2 true random none 0.001 2 true -1

[ks^-1] are thousands of nodes explored per second, lce [log] represents a log cache efficiency by a cache miss fraction (e.g., -3 means every 1000th CVRP calculation needed to be performed from scratch, the remaining were retrieved from cache), tph [ms] is the average time per such a CVRP calculation in milliseconds.

## Simulated annealing

> python SA.py --file ./insts/circ/circ4.txt --maxP 5 --maxC 10 --maxR 1 --T 400. --beta 0.999 --weight 4000 --teta 1.04
