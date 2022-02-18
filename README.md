# Forbid-Iterative (FI) Planner is an Automated PDDL based planner that includes planners for top-k, top-quality, and diverse computational tasks.

## The codebase consists of multiple planners, for multiple computational problems:

1. Top-k planning
2. Top-quality planning
3. Unordered top-quality planning
4. Satisficing/Agile diverse planning
5. Bounded diversity diverse planning
6. Bounded quality diverse planning
7. Bounded quality and diversity diverse planning
8. Bounded quality optimal diversity diverse planning

## The planners are based on the idea of obtaining multiple solutions by iteratively reformulating planning tasks to restrict the set of valid plans, forbidding previously found ones. Thus, the planners can be referred to as FI-top-k, FI-top-quality, FI-unordered-top-quality, FI-diverse-{agl,sat,bD,bQ,bQbD, bQoptD}.

The example invocation code can be found (for the corresponding computational problem) in
1. plan_topk.sh or plan_topk_via_unordered_topq.sh
2. plan_topq_via_topk.sh or plan_topq_via_unordered_topq.sh
3. plan_unordered_topq.sh
4. plan_diverse_{agl,sat}.sh
5. plan_diverse_bounded.sh
6. plan_quality_bounded_diverse_sat.sh
7. plan_quality_bounded_diversity_bounded_diverse.sh
8. plan_quality_bounded_diversity_optimal_diverse.sh

# Building
For building the code please use
```
./build.py release64
```

# Running
## FI-top-k
```
# ./plan_topk.sh <domain> <problem> <number-of-plans>
./plan_topk.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 1000
```
## FI-top-quality
```
# ./plan_topq_via_topk.sh <domain> <problem> <quality-multiplier>
./plan_topq_via_topk.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 1.1
```
## FI-unordered-top-quality
```
# ./plan_unordered_topq.sh <domain> <problem> <quality-multiplier>
./plan_unordered_topq.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 1.1
```
## FI-diverse-agl
```
# ./plan_diverse_agl.sh <domain> <problem> <number-of-plans>
./plan_diverse_agl.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 10
```
## FI-diverse-sat
```
## See the dependencies below (1 and 2)
# ./plan_diverse_sat.sh <domain> <problem> <number-of-plans> <diversity-metric> <larger-number-of-plans>
./plan_diverse_sat.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 10 stability 20
```
## FI-diverse-bD
``` 
## See the dependencies below (1, 2, and 3)
# ./plan_diverse_bounded.sh <domain> <problem> <number-of-plans> <diversity-metric> <bound> <larger-number-of-plans>
./plan_diverse_bounded.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 10 stability 0.25 20
```
## FI-diverse-bQ
``` 
## See the dependencies below (2)
# ./plan_quality_bounded_diverse_sat.sh <domain> <problem> <number-of-plans> <quality-bound>  <diversity-metric> <larger-number-of-plans>
./plan_quality_bounded_diverse_sat.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 10 1.1 stability 20
```
## FI-diverse-bQbD
``` 
## See the dependencies below (2 and 3)
# ./plan_quality_bounded_diversity_bounded_diverse.sh <domain> <problem> <number-of-plans> <quality-bound> <diversity-bound> <diversity-metric> <larger-number-of-plans>
./plan_quality_bounded_diversity_bounded_diverse.sh examples/logistics00/domain.pddl examples/logistics00/probLOGISTICS-4-0.pddl 10 1.1 0.1 stability 20
```


# Dependencies
For some of the diverse planners, the dependencies are as follows:
1. A Fast Downward (recent version) based planner for computing a single plan should be installed and a path to that planner should be specified in an environment variable **DIVERSE_FAST_DOWNWARD_PLANNER_PATH**. We suggest using the [*Cerberus* planner, post-IPC2018 version](https://github.com/ctpelok77/fd-red-black-postipc2018)
2. Computation of a subset of plans is performed in a post-processing, path to the code should be specified in an environment variable **DIVERSE_SCORE_COMPUTATION_PATH**. The code can be found [here](https://github.com/IBM/diversescore).
3. Note that for the diversity-bounded diverse planning the computation in a post-processing requires enabling CPLEX support in Fast Downward (see http://www.fast-downward.org/) and building the post-processing code with LP support.

For the unordered top-quality planner (and the planners that use it) that depend on finding a shortest cost-optimal plan, a Fast Downward (recent version) based shortest cost-optimal planner path should be specified in an environment variable **SHORTEST_OPTIMAL_FAST_DOWNWARD_PLANNER_PATH**. 

## Licensing

Forbid-Iterative (FI) Planner is an Automated PDDL based planner that
includes planners for top-k, top-quality, and diverse computational
tasks. Copyright (C) 2019  Michael Katz, IBM Research, USA.
The code extends the Fast Downward planning system. The license for the
extension is specified in the LICENSE file.

## Fast Downward
<img src="misc/images/fast-downward.svg" width="800" alt="Fast Downward">

Fast Downward is a domain-independent classical planning system.

Copyright 2003-2022 Fast Downward contributors (see below).

For further information:
- Fast Downward website: <https://www.fast-downward.org>
- Report a bug or file an issue: <https://issues.fast-downward.org>
- Fast Downward mailing list: <https://groups.google.com/forum/#!forum/fast-downward>
- Fast Downward main repository: <https://github.com/aibasel/downward>


## Tested software versions

This version of Fast Downward has been tested with the following software versions:

| OS           | Python | C++ compiler                                                     | CMake |
| ------------ | ------ | ---------------------------------------------------------------- | ----- |
| Ubuntu 20.04 | 3.8    | GCC 9, GCC 10, Clang 10, Clang 11                                | 3.16  |
| Ubuntu 18.04 | 3.6    | GCC 7, Clang 6                                                   | 3.10  |
| macOS 10.15  | 3.6    | AppleClang 12                                                    | 3.19  |
| Windows 10   | 3.6    | Visual Studio Enterprise 2017 (MSVC 19.16) and 2019 (MSVC 19.28) | 3.19  |

We test LP support with CPLEX 12.9, SoPlex 3.1.1 and Osi 0.107.9.
On Ubuntu, we test both CPLEX and SoPlex. On Windows, we currently
only test CPLEX, and on macOS, we do not test LP solvers (yet).


## Contributors

The following list includes all people that actively contributed to
Fast Downward, i.e. all people that appear in some commits in Fast
Downward's history (see below for a history on how Fast Downward
emerged) or people that influenced the development of such commits.
Currently, this list is sorted by the last year the person has been
active, and in case of ties, by the earliest year the person started
contributing, and finally by last name.

- 2003-2022 Malte Helmert
- 2008-2016, 2018-2022 Gabriele Roeger
- 2010-2022 Jendrik Seipp
- 2010-2011, 2013-2022 Silvan Sievers
- 2012-2022 Florian Pommerening
- 2013, 2015-2022 Salomé Eriksson
- 2018-2022 Patrick Ferber
- 2021-2022 Clemens Büchner
- 2021-2022 Dominik Drexler
- 2022 Remo Christen
- 2015, 2021 Thomas Keller
- 2016-2020 Cedric Geissmann
- 2017-2020 Guillem Francès
- 2018-2020 Augusto B. Corrêa
- 2020 Rik de Graaff
- 2015-2019 Manuel Heusner
- 2017 Daniel Killenberger
- 2016 Yusra Alkhazraji
- 2016 Martin Wehrle
- 2014-2015 Patrick von Reth
- 2009-2014 Erez Karpas
- 2014 Robert P. Goldman
- 2010-2012 Andrew Coles
- 2010, 2012 Patrik Haslum
- 2003-2011 Silvia Richter
- 2009-2011 Emil Keyder
- 2010-2011 Moritz Gronbach
- 2010-2011 Manuela Ortlieb
- 2011 Vidal Alcázar Saiz
- 2011 Michael Katz
- 2011 Raz Nissim
- 2010 Moritz Goebelbecker
- 2007-2009 Matthias Westphal
- 2009 Christian Muise


## History

The current version of Fast Downward is the merger of three different
projects:

- the original version of Fast Downward developed by Malte Helmert
  and Silvia Richter
- LAMA, developed by Silvia Richter and Matthias Westphal based on
  the original Fast Downward
- FD-Tech, a modified version of Fast Downward developed by Erez
  Karpas and Michael Katz based on the original code

In addition to these three main sources, the codebase incorporates
code and features from numerous branches of the Fast Downward codebase
developed for various research papers. The main contributors to these
branches are Malte Helmert, Gabi Röger and Silvia Richter.


## License

The following directory is not part of Fast Downward as covered by
this license:

- ./src/search/ext

For the rest, the following license applies:

```
Fast Downward is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or (at
your option) any later version.

Fast Downward is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.
```
