.. _DM_analysis:

********************
Dark Matter Analysis
********************

.. currentmodule:: gammapy.astro.darkmatter

Introduction
============

We are developing some simulation and analysis tools for gamma-ray dark matter
searches for different targets. The main aim is to provide a standardized
analysis pipeline. The corresponding functions and utilities are in
`gammapy.astro.darkmatter.spectra` and `gammapy.astro.darkmatter.utils`.

Simulations
-----------

The `gammapy.astro.darkmatter` module provides spatial and spectral models
for indirect WIMP dark matter searches (see `Bergstrom 2000`_ for a review).
This tools are aimed at people who already have some experience with dark matter
analysis.

The spatial distribution of dark matter halos is typically modeled with radially
symmetric profiles. Common profiles are the ones from
`Navarro, Frenk & White 1997`_ (NFW) or `Einasto 1965`_ for cuspy distributions
and an Isothermal (`Begeman, Broelis & Sanders 1991`_) or Burkert profiles
(`Burkert 1995`_) for cored dark matter distributions
(see `gammapy.astro.darkmatter.profiles`).

For the moment, all the simulation tools are for handling WIMP dark matter
annihilation case. It is under discussion to add the possibility for studying
also WIMP decaying dark matter.

To compute J-factors from the mentioned spatial models for a given potential
source, exists the class `gammapy.astro.darkmatter.JFactory`. This can be
used to obtain naive values for J-factors (no Jeans analysis or
possibility to account for substructure).

The spectral models in `gammapy.astro.darkmatter.PrimaryFlux` are based on
`Cirelli et al.  2011`_ (including EW corrections), which provides tabulated
spectra for different annihilation channels. These models are most commonly
used in VHE dark matter analyses.

Finally, the functionality to compute the expected gamma-ray flux coming from
an astrophysical object is in
`gammapy.astro.darkmatter.DarkMatterAnnihilationSpectralModel`.

Analysis
--------

The analysis pipeline is now working with simulated observations (also called
realizations) created using a
`gammapy.astro.darkmatter.DarkMatterAnnihilationSpectralModel` and combining it
with the IRFs of the telescope. This simulated observation is stored in
`gammapy.astro.darkmatter.DMDatasetOnOff`, which works with the On/Off
observation method. It is under discussion to open the analysis pipeline for
real observations. In the same lines, it is under development to include the
possibility to analyse two-dimensional spatial templates of the emission of the
sources. At the moment, the pipeline always assumes point-like sources.

The function that takes care of most of the analysis is
`gammapy.astro.darkmatter.SigmaVEstimator`. `SigmaVEstimator` is a complex
function, which last goal is to compute upper limits on the annihilation
cross-section in the case of not finding a DM signal in the simulated
observation.

To achieve this purpose, first we fit our observation to a list of
annihilation channels and DM particle masses. To perform the fit, we use a
binned Poisson likelihood (`wstat`_), which can be completed with terms for
nuisance parameters if selected. The included terms for nuisance parameters
enter in the likelihood as priors for its PDFs, following `Conrad 2015`_
for J-factors and `Aleksic, Rico & Martinez 2012`_ for the telescope
systematics. To obtain the maximum likelihood estimator for the cross-section
with physical meaning, we restrict the fit to the physical region
(positive values).

Once the fit is performed, `SigmaVEstimator` uses a profile likelihood ratio
test following `Rolke, Lopez & Conrad 2005`_ and `Cowan et al. 2011`_. Using a
significance of 5 sigma, the best fit is tested versus the null hypothesis. If
no positive signal is found, then we proceed to constraint the annihilation
cross-section searching for the corresponding 95% CL upper limits. This
method has been widely used before in the DM community to constraint the
cross-section upper limits (`Ackermann et al. 2011`_).

One last step is then needed because we are using simulated observations, so
this implies that in order to have statistical meaningful results we need to
repeat this process for O(100) realizations, and then compute the mean and
std for the upper limits, allowing to produce what is usually known as the
"Brazilian plot".

Reference/API
=============

.. automodapi:: gammapy.astro.darkmatter.profiles
    :no-inheritance-diagram:
    :include-all-objects:

.. automodapi:: gammapy.astro.darkmatter
    :no-inheritance-diagram:
    :include-all-objects:

.. automodapi:: gammapy.astro.darkmatter.spectra
    :no-inheritance-diagram:
    :include-all-objects:

.. automodapi:: gammapy.astro.darkmatter.utils
    :no-inheritance-diagram:
    :include-all-objects:

.. _Bergstrom 2000: https://iopscience.iop.org/article/10.1088/0034-4885/63/5/2r3
.. _Navarro, Frenk & White 1997: https://ui.adsabs.harvard.edu/abs/1997ApJ...490..493N/abstract
.. _Einasto 1965: https://ui.adsabs.harvard.edu/abs/1965TrAlm...5...87E/abstract
.. _Begeman, Broelis & Sanders 1991: https://ui.adsabs.harvard.edu/abs/1991MNRAS.249..523B/abstract
.. _Burkert 1995: https://iopscience.iop.org/article/10.1088/0034-4885/63/5/2r3
.. _Cirelli et al. 2011: http://iopscience.iop.org/article/10.1088/1475-7516/2011/03/051/pdf
.. _wstat: https://docs.gammapy.org/dev/api/gammapy.stats.wstat.html?highlight=wstat#gammapy.stats.wstat
.. _Conrad 2015: https://ui.adsabs.harvard.edu/abs/2015APh....62..165C/abstract
.. _Aleksic, Rico & Martinez 2012: https://ui.adsabs.harvard.edu/abs/2012JCAP...10..032A/abstract
.. _Cowan et al. 2011: https://ui.adsabs.harvard.edu/abs/2011EPJC...71.1554C/abstract
.. _Rolke, Lopez & Conrad 2005: https://ui.adsabs.harvard.edu/abs/2005NIMPA.551..493R/abstract
.. _Ackermann et al. 2011: https://ui.adsabs.harvard.edu/abs/2011PhRvL.107x1302A/abstract