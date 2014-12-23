SUNNY-CP
========

Parallel CP Portfolio Solver

sunny-cp tool allows to solve a Constraint (Satisfaction / Optimization) Problem 
defined in the MiniZinc language by using a portfolio approach.
It essentially implements the SUNNY algorithm described in [1][2][3].
sunny-cp is a parallel portfolio solver built on top of 12 constituent solvers:

  Choco, Chuffed, CPX, G12/LazyFD, G12/FD, G12/Gurobi, 
  G12/CBC, Gecode, HaifaCSP, iZplus, MinisatID, OR-Tools

In a nutshell, sunny-cp relies on two sequential steps:

  1. PRE-SOLVING: consists in the parallel execution of a static schedule and 
                  the feature extraction;
                  
  2. SOLVING: consists in the parallel execution of a number of predicted 
              solvers, selected by means of SUNNY algorithm.

This version is a major enhancement of the sequential sunny-cp portfolio solver 
described in [4], which attended the MiniZinc Challenge 2014 [5]. For more 
details about tool utilization, see the usage by typing sunny-cp --help.
sunny-cp solver was developed by Roberto Amadini and Jacopo Mauro (University of 
Bologna / Lab. Focus INRIA).
Most of its constituent solvers are publicly available, except for Chuffed and 
G12/Gurobi which have been kindly granted by NICTA Optimization Research Group.


PREREQUISITES
=============

sunny-cp is tested on 64-bit machines running Ubuntu 12.04, and not yet fully 
portable on other platforms. Some of the main requirements are:

+ Python 2.x
  https://www.python.org/

+ MiniZinc 1.6
  http://www.minizinc.org/

+ mzn2feat 1.0
  http://www.cs.unibo.it/~amadini/mzn2feat-1.0.tar.bz2

+ psutil 2.x
  https://pypi.python.org/pypi/psutil


CONTENTS
========

+ bin     contains the executables of sunny-cp

+ kb      contains the utilities for the knowledge base of sunny-cp

+ src     contains the sources of sunny-cp

+ solvers contains the utilities for the constituent solvers of sunny-cp

+ test    contains some MiniZinc examples for testing sunny-cp

+ tmp     is aimed at containing the temporary files produced by sunny-cp.


INSTALLATION
=============

Once downloaded the sources, move into sunny-cp folder and run install.sh:

  sunny-cp$ ./install.sh

This is a minimal installation script that checks the proper installation of 
Python (and psutil), MiniZinc, mzn2feat. In addition, it checks the installation 
of the constituent solvers (in the corresponding folder) and compiles all the 
python sources of sunny-cp. If the installation is successful, the following 
message will be printed on standard output:

  --- Everything went well!
  To complete sunny-cp installation you just have to add/modify the
  environment variables SUNNY_HOME and PATH:
  1. SUNNY_HOME must point to: "$PWD"/sunny-cp
  2. PATH must be extended to include: "$PWD"/bin"

It is important to set such variables in order to use sunny-cp. Once the 
variables are set, check the installation by typing the command: 

  sunny-cp --help

for printing the help page. Note that the installation process will also create 
the file SUNNY_HOME/src/pfolio_solvers.py containing an object for each 
installed solver (see below for more details about the constituent solvers).


SOLVERS
=======

This package does not contain neither the sources and the binaries of the 
constituent solvers, that should be installed separately.
Note that once a solver is installed on your machine, it is easy to add it to 
the portfolio and to customize its settings. For more details, see the README 
file in the SUNNY_HOME/solvers folder and the sunny-cp usage. A part of the 
solvers already included in MiniZinc 1.6, the other publicly available solvers 
used by sunny-cp are:

+ Choco 
  http://choco-solver.org/

+ Gecode
  http://www.gecode.org/

+ HaifaCSP
  http://tx.technion.ac.il/~mveksler/HCSP/index.html

+ iZplus
  http://www.constraint.org/ja/izc_download.html

+ MinisatID
  http://dtai.cs.kuleuven.be/krr/software/minisatid

+ OR-Tools
  https://code.google.com/p/or-tools/


FEATURES
========

During the presolving phase (in parallel with the static schedule execution) 
sunny-cp extracts a feature vector of the problem in order to compute the 
solvers schedule possibly run in the solving phase. By default, the feature 
vector is extracted by mzn2feat tool. sunny-cp provide the sources of this tool: 
for its installation, decompress the mzn2feat-1.0.tar.bz2 archive and follow the 
instruction in the README file of that folder. However, the user can define its 
own extractor by implementing a corresponding class in src/features.py


KNOWLEDGE BASE
==============

The SUNNY algorithm on which sunny-cp relies needs a knowledge base, that 
consists of a folder containing the information relevant for the schedule 
computation. The default sunny-cp knowledge base is in kb/all_T1800. It consists 
of 5527 CSP instances and 4988 COP instances. However, the user has the 
possibility of defining its own knowledge bases. For more details about the 
knowledge base management see the README file in SUNNY_HOME/kb folder.


TESTING
=======

In test/examples there is a number of simple MiniZinc models. You can run them 
individually, e.g.:

  SUNNY_HOME/test/examples:~$ sunny-cp zebra.mzn

or alternatively you can test all the models of the folder by typing:

  SUNNY_HOME/test/examples:~$ ./run_examples

The run_examples script also produces output.log and errors.log files in the 
SUNNY_HOME/test/examples folder, where the standard output/error of the tested 
models is respectively redirected.


FURTHER INFORMATION
===================

For any question or information, please contact us at:

  amadini at cs.unibo.it

  jmauro  at cs.unibo.it


ACKNOWLEDGMENTS
===============

We would like to thank the staff of the Optimization Research Group of NICTA 
(National ICT of Australia) for allowing us to use Chuffed and G12/Gurobi, as 
well as for granting us the computational resources needed for building and 
testing sunny-cp.


REFERENCES
==========

  [1]  R. Amadini, M. Gabbrielli, and J. Mauro. SUNNY: a Lazy Portfolio Approach 
       for Constraint Solving 2013. In ICLP, 2014.

  [2]  R. Amadini, M. Gabbrielli, and J. Mauro. Portfolio Approaches for 
       Constraint Optimization Problems. In LION, 2014.

  [3]  R. Amadini, and P.J. Stuckey. Sequential Time Splitting and Bounds 
       Communication for a Portfolio of Optimization Solvers. In CP, 2014.
      
  [4]  R. Amadini, M. Gabbrielli, and J. Mauro. SUNNY-CP: a Sequential CP 
       Portfolio Solver. In SAC, 2015.

  [5]  MiniZinc Challenge 2014.
       http://www.minizinc.org/challenge2014/results2014.html
