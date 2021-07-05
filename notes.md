In terms of performance, ALEX has a couple of known limitations:

Attack 1:

The premise of ALEX is to model the key distribution using a collection of linear regressions. Therefore, ALEX performs poorly when the key distribution is difficult to model with linear regressions, i.e., when the key distribution is highly nonlinear at small scales. A possible future research direction is to use a broader class of modeling techniques (e.g., also consider polynomial regression models).

Attack 2:

ALEX can have poor performance in the presence of extreme outlier keys, which can cause the key domain and ALEX's tree depth to become unnecessarily large (see Section 5.1 of our paper). A possible future research direction is to add special logic for handling extreme outliers, or to have a modeling strategy that is robust to sparse key spaces.