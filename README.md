# Linear-Regression-on-LISA-SNR-Data
**Paolo Marcoccia<sup>1</sup>**

<sub>1. University of Stavanger, Institutt for Matematikk og Fysikk, Kjølv Egelands hus, 5.etg, E-blokk, 4021 Stavanger, Norway </sub>   

## Abstract ##

We use Linear Regression to try to interpolate the SNR on the LISA detector strain for a given set of Stellar Origin Binary Blach Holes Merging Events.
To fit the model, we decided to use as features respectively Chirp Mass, Mass difference, Distance, In Band time in the LISA frequency range and Inclination.
In particular, we used a function for Chirp Mass, Distance and Inclination in order to improve the efficiency of the fit, as from theory we already know the correct mathematical dependancy on these parameters that the SNR of a Black Hole Merging signal has.
We find that a simple Linear Regression is not able to interpolate exhaustively this kind of task, resulting in a average percentual error on both train data and test data around 90%, the reasons for this outcome are several.
To begin, we know that the dependancy of the SNR on the LISA strain is highly nonlinear and generically depends on many more parameters;
furthermore, we used a real run simulation of the LISA observation as training Data, and as we know that loud events having high SNR are much less likely to appear compared to events with low SNR, which makes the train data non-optimal in order to understand the behaviour of events having high SNR on the LISA strain.

## Introduction ##

_Gravitational wave[GW]_ science became on the last years the leading trend when speaking about astrophysics and cosmology, this is mainly due to the fact that _Gravitational Waves[GWs]_ signals allowed mankind to observe properties of the universe and its most exotic object that where not possible to obtain by any other mean.
In addition to being the most recent validity test for [Einstein's General Relativity](https://arxiv.org/abs/1612.09309), gravitational waves can be used to test highly compact object of our universe like _Black Holes[BH]_ and _Neutron stars[NS]_, which at the current state of art still have a lot of open questions on their structure that can only be answered by _GW_ science.
Unfortunately, the power of this new tool comes at a high price, the signals on the detectors have extremely small amplitude compared to their noise, hence optimal data analysis techniques are needed in order to dig them out of the strain.
Among the most used method to find the signals on the detector strain there is the [matched filtering](https://arxiv.org/abs/gr-qc/9808076), which consist in matching the strain with several billions of _GWs_ template, in order to find a corrispondence in the strain with theoretically built _GWs_ signals and their amplitude in the strain compared to the noise described by their value of _Signal to Noise Ratio [SNR]_.
We don't have to emphasize that this procedure is indeed a computationally costly one, both during the search and during the creation of the template banks, for which approximated techniques were developed in order to avoid integrating the highly non-linear [Einstein equations].
This procedure is even more complicated in the case of [LISA](https://arxiv.org/abs/1702.00786), where due to its choice of frequency range several _BH_ merging events appear in the same time on the strain.
The use of _AI_ can hence be a valuable tool in improving the performance of pipelines in _GW_ science, in [this](https://github.com/KuZa91/Linear-Regressionon-LISA-SNR/blob/main/SNRLinearRegression.ipynb) notebook, in particular, we are gonna try to estimate by using a Linear Regression the _ideal SNR_( i.e. the _SNR_ without considering the noise due to the other events that appear in the LISA strain) of the events that would appear on the strain given a set of parameters that describe them.

## Data and Analysis Details ##

Our training data, would be generated using the notebook at the [link](https://github.com/KuZa91/Generating-a-BH-Merging-Catalogue/blob/master/BHCatalogV4.1.ipynb), which generates a _Stellar origin binary Black Hole_ population following the statistical inferred properties by [LIGO](https://arxiv.org/pdf/2010.14533.pdf).
The generated catalogue, is then post-processed by the LISA non-open software, which was developed by _S. Babak, N. Karnesis et al._. in order to obtain the SNR for the single events of the catalogues.
