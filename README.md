# rmafkernel

This is an implementation in Java of the kernelization algorithm described
in "Cyclic generators and an improved linear kernel for the rooted subtree
prune and regraft distance" by Kelk, Linz and Meuwese,
Information Processing Letters, Volume 180, February 2023, 106336. 
https://doi.org/10.1016/j.ipl.2022.106336

It is a kernelization algorithm for the rooted maximum agreement forest
(rMAF) problem on two rooted binary trees on the same set of taxa X.

It applies the reduction rules described in the article (subtree, chain,
3-2) to exhaustion.

COMPILATION
===========

To compile it you need javacc (the Java compiler-compiler) and javac. 

javacc RMAFKernel.jj

javac RMAFKernel.java

I have also made an executable JAR file RMAFKernel.jar available, should you not
wish to compile from source.

EXECUTION
=========

You simply feed a file consisting of two lines into the standard input. On each
line there is a one rooted binary phylogenetic tree, in the usual Newick format.

There is an optional switch "--verbose" which gives you slightly more detailed
information about which reduction rule is executing and when.

Whether you use the --verbose switch or not, the output consists of lines that
begin with "//" (which is auxiliary information) followed by the last three
lines, without a prepended "//", which consist of:
1. the first reduced tree
2. the second reduced tree
3. the parameter adjustment k. This is the number of 3-2 reductions that were
executed: each such execution reduces the size of the optimum by exactly 1.
Hence, after solving the reduced trees, you should k back to it to get the
optimum for the original pair of trees.

Here is an example execution, where example.tree is the pair of trees with 350 taxa
from the Github repository.

java -jar RMAFKernel.jar < example.tree > example.out

For more verbose output:

java -jar RMAFKernel.jar --verbose < example.tree > example.outverbose

You will see that the reduced trees have 47 taxa, and that the parameter has been
adjusted by 21. So adding 21 to the optimum solution for the 47-taxa trees, gives
you the optimum for the original pair of trees.

TECHNICAL NOTES
===============
1. The code does not introduce \rho. So if you are working with the extremely
closely-related rSPR distance, as opposed to rMAF, please take that into account.

2. Rather than truncating maximal common chains to length 3, the code simply iteratively
truncates maximal common chains of length 4 to length 3.

3. If you have solved the reduced pair of trees, and which to transform this agreement
forest into an agreement forest for the original pair of trees, please be aware of
subtleties around the 'undoing' of the chain reduction. Specifically: if there is a
common chain (a,b,c,d) and taxon 'd' is cut off during the kernelization, and you wish
to obtain an agreement forest for the instance with d, from an agreement forest for
the instance without, you need to first ensure that {a,b,c} are all in the same
agreement forest component. (If they are not, you can modify the agreement forest
to achieve this, without increasing its size).


RELEASE NOTES
=============
The first release (v1.0) was 27th December 2025.


BUGS ETC?
=========
Just let me know! Cheers, Steven.
