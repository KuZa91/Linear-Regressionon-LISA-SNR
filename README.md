
# Building AI course project - Linear-Regression-on-LISA-SNR-Data
**Paolo Marcoccia<sup>1</sup>**

<sub>1. University of Stavanger, Institutt for Matematikk og Fysikk, Kjølv Egelands hus, 5.etg, E-blokk, 4021 Stavanger, Norway </sub>   

## Summary ##

We use Linear Regression to try to interpolate the SNR on the LISA detector strain for a given set of Stellar Origin Binary Blach Holes Merging Events.
To fit the model, we decided to use as features respectively Chirp Mass, Mass difference, Distance, In Band time in the LISA frequency range and Inclination.
In particular, we used a function for Chirp Mass, Distance and Inclination in order to improve the efficiency of the fit, as from theory we already know the correct mathematical dependancy on these parameters that the SNR of a Black Hole Merging signal has.
We find that a simple Linear Regression is not able to interpolate exhaustively this kind of task, resulting in a average percentual error on both train data and test data around 90 %, the reasons for this outcome are several.
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
We then decided to only keep the most relevant parameters to train the Linear Regression, in particular the parameters used are :

- _1/Distance_ : The most important parameter influencing the SNR of a source on the detector strain, if the distance of the source is small enough, the SNR would increase regardless of all the other involved parameters;

- _Chirp Mass^(5/2)_ : Together with distance, one of the main parameters in defining the resulting SNR on the detector strain, can be easily obtained from the two masses of the black holes involved in the merging using the function declared at _In[10]_ of the notebook at the [link](https://github.com/KuZa91/Generating-a-BH-Merging-Catalogue/blob/master/BHCatalogV4.1.ipynb). We decided to use this parameter to the power of $5/2$ as we know from literature that it's the standard SNR dependance on it;

- _Mass Difference_ : Simply given by the difference between _m1_ and _m2_, even though it's not a main parameter for the resulting _SNR_ high difference can drastically affect the inspiral phase of the merging, and it's resulting signal on the detector;

- _In Band Time_ : We know that the amplitude of a _GW_ signal is bigger the more the event is closest to its coalescing phase, and the frequency at which the source produce _GWs_ also increase in function of its time evolution. Furthermore, events with larger masses moves toward the coalescing phase faster than the ones with lower masses. We hence decided to use the _In Band Time_, that is the time that the event will spend in the LISA frequency range (i.e. 10⁻4 to 0.1 Hz), both to have an index on how close the event is to merging plus additional information on the mass of the events. We decided to not use the _initial frequency_ at which the events enter in the LISA band because, as stated before, the amount of time needed to coalesce will still be a function of the masses;

- _Inclination Factor_ : In the case of a source orthogonal to the detector position, the amplitude of _GWs_ is deeply suppressed, we hence generated the inclination in order to maximize its value when close to _pi / 2_, as a linear regression cannot get the dependancy on this parameter (which is symmetric around _pi / 2_) by itself. We started by defining the distance of the inclination from said value as _di_, we then decided to build the _i-factor_ simply as _1/(1 - exp(-di))_ .  

To conclude, we added an array of _1_ to the train data in order to fit an intercept to the model, a lite dataset to test the code can be found in the [github directory](https://github.com/KuZa91/Linear-Regressionon-LISA-SNR/blob/main/TrainDataLite.h5).
In the proper [run](https://github.com/KuZa91/Linear-Regressionon-LISA-SNR/blob/main/SNRLinearRegression.ipynb), it was used a training set with around 35 millions events, of which 9/10 of the total were used as training data and the rest as test data.

## Results and possible improvements ##

After the analysis, we observed that the efficiency of the method is pretty low, with around 90% deviation from the real results both on the estimation of the train data and on the test data. This is mainly due to 2 things :

- The dependancy of the SNR on the parameters we used is higly non-linear and is influenced by several other parameters, the best approach to the problem would probably be a _Neural Network_, which however requires much more resources to build than this straightforward implementation;

- The dataset used to train the data comes out of a proper realization of what LISA would observe during a run, and by nature, we would have much more events that are extremely weak and few loud sources. This is indeed not the best dataset to train the method, as we would miss informations on the behaviour of signals with high SNR while overfitting signals with low amplitude. 


