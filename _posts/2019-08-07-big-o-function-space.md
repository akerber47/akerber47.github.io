One fun fact / alternative perspective that occurred to me as I was working
through some elementary problems on big O notation: if viewed as a class of
functions, each big-O set {f(x)|f(x)=O(g(x))} is a linear subspace of the
function space under consideration. For instance, if we're looking only at
polynomials of degree <= n, as subspaces O(1) in O(x) in O(x^2) in ...  gives
us a flag of subspaces. I suppose one could use this to draw pictures of the
sets of functions that satisfy certain relations. I wonder if this would be
helpful to explain the concept, or if it just makes things more confusing.

It would be great if this told us something cool for more general spaces of
functions, but of course they're all infinite-dimensional so it's hard to say
much about what's happening, other than that I suspect it's a drastically
reduced subset. It's hard to think of a nice map either way.  (If we're being
silly about things, the set of functions that actually come up in CS theory is
basically finite-dimensional anyways!)

Note: big theta is a bit difference as it's more like equivalence relations
/ equivalence classes than it is like a linear subspace.

Disclaimer: I'm not really sure this is right/sensible. Usually we only talk
about big O for the positive side of things, so I'm using a somewhat
vague/made-up more general definition that probably just boils down to some
sort of Lipschitz condition. Please forgive me as this is all speculative and
I'm not an analyst by training.
