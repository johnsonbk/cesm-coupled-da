####################
SMYLE initialization
####################

The `Seasonal-to-Multiyear Large Ensemble (SMYLE) <https://www.cesm.ucar.edu/events/wg-meetings/2021/files/system/Yeager.pdf>`_ seeks to use a CESM B Compset on the ~1.0° ``f09_g17``
finite-volume grid in order to make multi-year predictions.

It's important to note that the peer-reviewed papers used to justify the SMYLE
effort use anomaly correlations to support the notion that extented forecasts
have meaningful predictive value:  Luo et al. (2008), [1]_ Dunstone et al.
(2016), [2]_ DiNezio et al. (2017), [3]_ Lovenduski et al. (2019), [4]_
Dunstone et al. (2020), [5]_ and Esit et al. (2021). [6]_

Anomaly correlations are distinct from the skill scores used historically in
studies of prediction skill. The skill scores that have been used by the
numerical prediction community exhibit two features:

1. they make an explicit prediction
2. they estimate an error in that prediction.

For example, the :math:`S_1` score compares the error in the forecasted 500 hPa
pressure surface against the magnitude of the horizontal pressure gradient
(Teweles and Wobus, 1954 [7]_ ). The height of the 500 hPa pressure is an
explicit prediction of the future state of the atmosphere and the horizontal
gradient "normalizes" in a sense, the magnitude of the error, since the error
should be larger in areas where gradients are large.

Skill scores of this type are meaningful and useful -- the National Centers for
Environmental Prediction have tracked the operational :math:`S_1` score
throughout the history of the center since it effectively tracks the
improvement predictive skill through several scientific generations.

Before devoting considerable time to the SMYLE effort, note that:

1. anomaly correlations aren't predictions,
2. strong, spatially coherent correlations on interannual timescales can be
   observed even when the "signal" that is being correlated is synthetic noise
   (Livezey and Chen, 1983 [8]_ ), and
3. anomaly correlations aren't predictions (this bears repeating).

References
==========

.. [1] Luo et al., 2008: Extended ENSO Predictions Using a Fully Coupled
       Ocean–Atmosphere Model, *J Clim*, **21(1)**, 84–93, https://doi.org/10.1175/2007JCLI1412.1.
.. [2] Dunstone et al., 2016: Skilful predictions of the winter North Atlantic
       Oscillation one year ahead, *Nat Geosci*, **9**, 809–814, https://doi.org/10.1038/NGEO2824.
.. [3] DiNezio et al., 2017: A 2 Year Forecast for a 60–80% Chance of La Niña
       in 2017–2018, *GRL*, **44(22)** 11,624-11,635,  https://doi.org/10.1002/2017GL074904.
.. [4] Lovenduski et al., 2019: Predicting near-term variability in ocean
       carbon uptake, *Earth Syst Dynam*, **10**, 45–57, https://doi.org/10.5194/esd-10-45-2019.
.. [5] Dunstone et al., 2020: Skilful interannual climate prediction from
       two large initialized model ensembles, *ERL*, **15(9)**,  https://doi.org/10.1088/1748-9326/ab9f7d.
.. [6] Esit, M., S. Kumar, A. Pandey, D. M. Lawrence, I. Rangwala, and S. 
       Yeager, 2021: Seasonal to multi-year soil moisture drought forecasting.
       *npj Clim Atmos Sci*, **4**, 1–8, https://doi.org/10.1038/s41612-021-00172-z.
.. [7] Teweles, S., and H. B. Wobus, 1954: Verification of Prognostic Charts.
       *Bul Am Meteor Soc*, **35**, 455–463, https://doi.org/10.1175/1520-0477-35.10.455.
.. [8] Livezey, R. E., and W. Y. Chen, 1983: Statistical Field Significance and
       its Determination by Monte Carlo Techniques. *Mon Wea Rev*, **111**,
       46–59, `doi:10.1175/1520-0493(1983)111<0046:SFSAID>2.0.CO;2 <https://doi.org/10.1175/1520-0493\(1983\)111\<0046\:SFSAID\>2.0.CO;2>`_.
