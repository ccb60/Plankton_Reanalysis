### Understanding Differences Between the two Log Models
The following is my effort to understand the differences between these two 
models.  These are really notes for myself.

#### Traditional Loglinear model
A Gausian GLM / GAM model on long-transformed data fits the following model:

$$ log(Y) = \beta_1 X_1 + \beta_2 X_2 + \ldots + \epsilon$$

$$Y = exp(\beta_1 X_1 + \beta_2 X_2 + \ldots + \epsilon)$$


$$Y = exp(\beta_1 X_1) \times  exp(\beta_2 X_2) \times \ldots \times exp(\epsilon)$$

Where $\epsilon$ is distributed as a normal variate.  The predictors enter the 
model in a multiplicative fashion,  the error is a lognormal error, and it also 
enters the model in a multiplicative fashion.

In practice, a lognormal distribution allows predictions to differ more from
high observations than low ones (because the error it "sees" in the difference
between a log of the prediction and the log of the observation).  Since logs
climb slowly as each observations get higher, a similar sized deviation between
predicted and observed is given more weight if the prediction is for a low 
density than if the prediction is for a high density.

#### Gaussian Model with Log Link
A Gaussial GLM on untransformed data using a log link fits a slightly different 
model, defined as follows:

$$ log(E(Y)) = \beta_1 X_1 + \beta_2 X_2 + \ldots $$

$$ E(Y) = exp(B_1X_1) \times exp(\beta_1X_1) \times \ldots$$

$$ log(E(Y)) = \beta_1 X_1 + \beta_2 X_2 + \ldots $$


$$ E(Y) = exp(B_1X_1) \times exp(\beta_2X_2) \times \ldots$$

$$ Y = exp(B_1X_1) \times exp(\beta_2X_2) \times \ldots + \epsilon $$

So this model also assumes the predictors enter the model in a multiplicative 
manner, but the errors are normally distributed.  In essence, this means the
model "sees" deviations (in a RMSE sense) from high predictions as just as 
important as deviations from low predictions.  It is this different weighting of
errors that causes the models to differ in practice.

My guess is that the differences between these two models would not be as 
evident if we were looking at simpler, linear models (GLMs) instead of GAMS.  
The problem is in part that the GAM "wants" to fit every wiggle, and the 
Gaussian model with a log link tries to fit "wiggles" at high values just
as closely as "wiggles" at low values.

