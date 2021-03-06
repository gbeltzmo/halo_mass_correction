# README

These are halo mass corrections based on the Illustris, IllustrisTNG, and EAGLE simulations.

If you make use of the halo mass corrections in this repository, we ask that you please cite [Beltz-Mohrmann, G. D. & Berlind, A. A., "The impact of baryonic physics on the abundance, clustering, and concentration of halos", 2021, submitted to The Astrophysical Journal](https://ui.adsabs.harvard.edu/abs/2021arXiv210305076B/abstract).

After cloning the repository you must add it to your PYTHONPATH.

The corrections are 7th order polynomials, and the coefficients are given in order from lowest to highest, i.e. h,g,f,e,d,c,b,a.

The corrected DMO halo masses (in units of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>) are given by

M<sub>h,corrected</sub> = (y + 1) M<sub>h,DMO</sub>

where M<sub>h,DMO</sub> is the unlogged original halo mass in units of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>, and

y = ax<sup>7</sup> + bx<sup>6</sup> + cx<sup>5</sup> + dx<sup>4</sup> + ex<sup>3</sup> + fx<sup>2</sup> + gx + h 

where x = log<sub>10</sub>(M<sub>h,DMO</sub>) and a through h are the polynomial coefficients for a given simulation, halo definition, redshift, and environment.

The code takes 5 arguments: halo masses (nd.array), environment ('all', 'high', 'low'), halo definition ('200b', 'fof', 'vir', '200c', '500c'), redshift (0,1,2), and simulation ('illustris', 'tng', 'eagle'). The code returns an array of corrected halo masses. 

If you are interested in the halo mass correction for a redshift not given here, please contact gillian.d.beltz-mohrmann@vanderbilt.edu.

#### Example usage:

```
import numpy as np
import halo_mass_correction as hmc

masses = np.array([1e14, 1e13, 1e12, 1e11, 1e10])

corrected = hmc.correction(halo_masses=masses, env='all', halo_def='200b', redshift=0, sim='tng')
```

The code has a lower mass limit of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>. Additionally, each environment/halo definition/redshift/simulation combination has an upper mass limit. If a given halo mass is outside the acceptable mass range, the code will issue a warning, and return the original (uncorrected) halo mass for that halo.

If the environment provided is 'all' the code expects to take in an array of all halos, regardless of their environment. If the environment provided is 'high' the code expects to take in an array of only halos in high-density environments. Likewise, if the environment provided is 'low' the code expects to take in an array of only halos in low-density environments. 

Our halo environment measure is the mass (in halos) in 5 Mpc spheres centered on each halo of interest. (For this, we measure the environment on all DMO halos above 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>, and on all hydrodynamic halos above 8 x 10<sup>9</sup> h<sup>-1</sup> M<sub>&#9737;</sub>.) Within the 5 Mpc sphere, we do not sum up all particles, but rather sum up all the mass within halos (of any size). 

We can also define an environment factor &delta; for each halo, such that &delta; = (&delta;<sub>sphere</sub> / &delta;<sub>box</sub>) - 1, where &delta;<sub>sphere</sub> is the mass in a 5 Mpc sphere around the halo divided by the volume of a 5 Mpc sphere, and &delta;<sub>box</sub> is the sum of all halo masses in the box divided by the volume of the box.

The median environment (mass) as well as the median &delta; used to split halos into high and low density environments for each simulation/halo definition/redshift are given in halo_environments.txt.
