# Project Suggestions

These projects are intentionally open-ended.  They are intended as an
opportunity for you to showcase your mastery of the learning goals:

* Parallel algorithmic reasoning.

* Parallel cost models.

* Judging the suitability of the language/tool for the problem at
  hand.

* Applied data-parallel programming.

This means that you are free to diverge from the project descriptions
below, or come up with your own ideas, as long as they provide a
context in which you can demonstrate the course contents.

You are *not* judged on whether e.g. Futhark or ISPC or whatever
language you choose happens to be a good fit or run particularly fast
for whatever problem you end up picking, but you *are* judged on how
you evaluate its suitability.

## Porting PBBS benchmarks

The [Problem Based Benchmark
Suite](https://cmuparlay.github.io/pbbsbench/) is a collection of
benchmark programs written in parallel C++. We are interested in
porting them to a high-level parallel language (e.g. Futhark). Some of
the benchmarks are relatively trivial; others are more difficult. It
might be a good idea for a project to combine a trivial benchmark with
a more complex one. The [list of benchmarks is
here](https://cmuparlay.github.io/pbbsbench/benchmarks/index.html).
The ones listed as *Basic Building Blocks* are all pretty
straightforward. Look at the others and pick whatever looks
interesting (but talk to us first - some, e.g. rayCast, involve no
interesting parallelism, and so are not a good DPP project).
Particularly interesting to Troels are the ones related to
computational geometry:

* [delaunayRefine](https://cmuparlay.github.io/pbbsbench/benchmarks/delaunayRefine.html)
* [delaunayTriangulation](https://cmuparlay.github.io/pbbsbench/benchmarks/delaunayTriangulation.html)
* [rangeQuery2d](https://cmuparlay.github.io/pbbsbench/benchmarks/rangeQuery2d.html)

## Project Related to Automatic Differentiation

[Minpack-2](material-projects/Mpack-2/Minpack-2.pdf) is a collection
of problems that require computation of derivatives. The
implementation language is Fortran, and each problem implementation
has options for computing the primal (original program), or/and the
associated Jacobian (or even Hessian).

This task refers to porting one (or several) of the Minpack-2
benchmarks to Futhark: you need to translate only "the primal" (i.e.,
the original function that requires differentiation), and then you may
use Futhark's support for automatic differentiation to compute the
dense Jacobian/Hessians.

Many of the Minpack-2 primals result in sparse Jacobians or Hessians
(i.e., the second-order derivative); hence the last step is to
visualize/characterize the sparsity of the differentiated code. [Here
is a paper that shows the sparsity of a several applications from
Minpack-2](material-projects/Mpack-2/Efficient_Computation_of_Gradients_and_Jacobians_b.pdf)

A "project outside the scope" with the same goal, but which did not
reach the visualization goal is available
[here](https://futhark-lang.org/student-projects/peter-msc-project.pdf);
perhaps you will find it useful at least for the Minpack-2 related
information (inside).

Bonus: if time permits, you may try to optimize the computation, e.g.,
by packing in a safe way several unit vectors into a denser
representation that contains several one entries.

## Implementing full flattening in the Futhark compiler

We are currently working on implementing full flattening in the
Futhark compiler. [The current state of the implementation can be seen
here.](https://github.com/diku-dk/futhark/blob/flattening/src/Futhark/Pass/Flatten.hs).
The current implementation is very incomplete, but can handle some
programs. This project is about implementing *simple* missing cases in
the flattening transformation. Ideas (one of these is probably
enough):

* The general case of slicing is implemented, but one could implement
  a more efficient version of the case where the *size* is invariant
  to the `map` and only offset/stride is variant.

* Implementing segmented concat.

* Implementing irregular segmented scan.

* Implementing segmented histograms (only a partial implementation is
  realistic).

This project involves hacking on the actual Futhark compiler, which is
written in Haskell.  Further, it is designed to be a *good compiler*,
rather than to be immediately accessible to students.  However, you
can expect significant help in figuring out the compiler side of
things.  This is a *tricky* project, which is why what is expected of
you is relatively small.  You might start by first implementing (in
normal Futhark) an example of the code you want, and then afterwards
implementing it in the compiler.

This kind of project is most interesting to people who wish to do more
Futhark-related projects later in their studies.

## Batched Rank-k Search

For those that have not chosen this problem in the PMPH course, you are welcome to solve [the rank-k search problem](material-projects/rank-search-k/Project-RankSearch-k.pdf).

## Reverse-AD Improvements applied to the Futhark Compiler

This project refers to implementing the reverse-AD differentiation of scans by means of the rule presented in the PPAD paper [1], for the cases when the scan operator is defined on scalar tuples of arity at least two. 

Furthermore, as you have probably observed in the solutions to your weekly-4, there is some overhead associated to the reverse-AD imlementation of reduce-by-index with multiplication as operator. You can try to eliminate the observed overhead as well.

*If time permits*, you may study as well a solution for enabling Futhark to take advantage of invertible operators, e.g., efficiently supporting the case of reduce or reduce-by-index with the sum-of-product operator. In principle this can be accomplished by using attributes to annotate the source call to a reduce or reduce-by-index/hist with the names of the meta functions required by the translation (such as the forward/backward conversions, lifted and invertible operator, etc.). Once you can identify the meta functions, it is relatively straightforward to generate the efficient code.

The Futhark compiler code that is relevant to this project is rooted in file `/futhark/src/Futhark/AD/Rev/SOAC.hs`; from there you may follow the different instances of second-order array combinators (SOACs), for example the reverse-AD treatment of `scan` in `Scan.hs` or of `reduce` in `Reduce.hs` or of `reduce-by-index` in `Hist.hs`.

[1] A. Paszke, M. J. Johnson, R. Frostig, and D. Maclaurin, "Parallelism-Preserving Automatic Differentiation for Second-Order Array Languages", FHPNC, 2021.
