# kelvin-power-challenge

4th place solution of [Kelvin Mars Power Challenge](https://kelvins.esa.int/mars-express-power-challenge/).

## Flowchart

<img src="./fig/workflow.png" alt="Flowchart" align="center" width="900px"/>


## Features

####EVTF:
- Indicators for MARS PENUMBRA and UMBRA
- Interpolated altitude over Mars from APOCENTER and PERICENTER

####SAAF:
- All raw variables
- All raw variables at previous hour
- All variables averaged over previous 3 and 24 hours
 
####LTDATA:
- All raw variables

####FTL:
- Indicator variable per type
- Indicator variable for comms enabled

####DMOP:
- Count of commands per subsystem/hour
- Sum of all commands per hour

####Custom features:
- Indicator for solar conjunction
- Indicator for periods with missing data
- Indicator for switching of NPWD2851 based on manual inspection of DMOP commands
- Days since 2008

All features are calculated or downsample to the hour. 

## Modeling

For modeling, I used gradient boosting trees and LSTM using the excellent packages xgboost and keras. The predictions from the neural nets differed substantially from the predictions of the tree-based models. The best ensemble was built using a library of models trained on random subsets of features (80%) and ensembled using Caruana's greedy ensemble selection method (also known as hillclimbing). 


## Cross validation scheme

<img src="./fig/cv_scheme.png" alt="CV Scheme" align="center" width="900px"/>


## Models and performance
| Model | Public Leaderboard RMSE | Position |
| :-------------------------: | :-----------------------: | :---------------: | 
| Gradient Boosting Trees (Xgboost) | 0.09015 | **6** |
| Long-term Short-term (LSTM) recurrent neural nets (Keras) | 0.09096 | **6** |
| Deep neural nets (Keras) | 0.09045 | **6** |
| Best Ensemble (200 models) | 0.08464 | **4** |


## Instructions

#### Step 1. Install dependencies
- Python 2.7
- Numpy
- Pandas 0.18 (for older verisons you need to change .resample() methods to old syntax)
- Scikit-learn 0.17
- Keras 1.04
- Xgboost 0.4

A convenient way to get all prerequisites is to install the [Anaconda](https://www.continuum.io/downloads) Python distribution. Use the script in env folder for automated download and install of anaconda dependencies.

####Step 2. Download source and competition data

1. Clone the repository using git: `git clone https://github.com/alex-bauer/kelvin-power-challenge.git`
2. Download data file from [competition website](https://kelvins.esa.int/mars-express-power-challenge/data/) and unpack to data folder

####Step 3. Best single models: Generate features and predict using single models (Xgboost, LSTM, DNN, ETR)

3. Run generate_features.sh
4. Run generate_single_models.sh

####Step 3. Generate model library and build ensemble using Caruana's greedy ensemble selection methods

5. Run build_ensemble.sh (this can take several days to finish, depending on your hardware)

#### Baseline solutions

Mean baseline:
Run `python mean_baseline.py`

Random Forest baseline:
4. Run `python rf_baseline.py` (0.12 on public leaderboard)


## Acknowledgements

Many thanks to ESA for organizing the competition and providing the data, and many thanks to Sebastian Apel who organized workshops in Frankfurt to discuss ideas and interact with the ESA mission control team.
