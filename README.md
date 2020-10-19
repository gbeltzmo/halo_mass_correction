## README

Halo mass corrections based on Illustris, IllustrisTNG, and EAGLE from Beltz-Mohrmann et al. (2020).

The corrections are 7th order polynomials, and the coefficients are given in order from lowest to highest, i.e. h,g,f,e,d,c,b,a.

The corrected DMO halo masses (in units of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>) are given by

M<sub>h,corrected</sub> = (y + 1) M<sub>h,DMO</sub>

where M<sub>h,DMO</sub> is the unlogged original halo mass in units of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>, and

y = ax<sup>7</sup> + bx<sup>6</sup> + cx<sup>5</sup> + dx<sup>4</sup> + ex<sup>3</sup> + fx<sup>2</sup> + gx + h 

where x = log<sub>10</sub>(M<sub>h,DMO</sub>) and a through h are the polynomial coefficients for a given simulation, halo definition, redshift, and environment.


The code takes 5 arguments: halo masses (nd.array), environment ('all', 'high', 'low'), halo definition ('200b', 'fof', 'vir', '200c', '500c'), redshift (0,1,2), and simulation ('illustris', 'tng', 'eagle'). The code returns an array of corrected halo masses. 

The code has a lower mass limit of 10<sup>10</sup> h<sup>-1</sup> M<sub>&#9737;</sub>. Additionally, each environment/halo definition/redshift/simulation combination has an upper mass limit. If a given halo mass is outside the acceptable mass range, the code will issue a warning, and return the original (uncorrected) halo mass for that halo.

If the environment provided is 'all' the code expects to take in an array of all halos, regardless of their environment. If the environment provided is 'high' the code expects to take in an array of only halos in high-density environments. Likewise, if the environment provided is 'low' the code expects to take in an array of only halos in low-density environments. The median environments used for each simulation/halo definition/redshift are given in halo_environments.txt.

#### Example usage:

<code>
import numpy as np
import halo_mass_correction as hmc

corrected = hmc.correction(halo_masses=np.array([1e14, 1e13, 1e12, 1e11, 1e10]), env='all', halo_def='200b', redshift=0, sim='tng')
</code>
