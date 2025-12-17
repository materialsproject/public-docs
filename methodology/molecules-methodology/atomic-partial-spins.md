---
description: How partial spins for open-shell molecules are determined in MPcules
---

# Atomic Partial Spins

Atomic partial spins can be defined similarly to atomic partial charges for molecules with unpaired electrons ("open-shell" molecules; "closed-shell" molecules with no upaired electrons have 0 net spin and therefore 0 partial spin on each atom, by definition). We currently calculate atomic partial spins using two methods: Mulliken population analysis \[1] and natural atomic populations from NBO \[2, 3]. While Mulliken partial charges can be unreliable, we have generally found that Mulliken partial spins are qualitatively similar to those obtained from NBO and can therefore be treated without prejudice.

## References

1.  Mulliken, R.S., 1955. Electronic population analysis on LCAO–MO molecular wave functions. I. _The Journal of chemical physics_, _23_(10), pp.1833-1840.
2. Glendening, E.D., Badenhoop, J.K., Reed, A.E., Carpenter, J.E., Bohmann, J.A., Morales, C.M., Karafiloglou, P., Landis, C.R., Weinhold, F., 2018. _NBO 7.0_. Theoretical Chemistry Institute, University of Wisconsin, Madison.
3. Glendening, E.D., Landis, C.R. and Weinhold, F., 2012. Natural bond orbital methods. _Wiley interdisciplinary reviews: computational molecular science_, _2_(1), pp.1-42.
