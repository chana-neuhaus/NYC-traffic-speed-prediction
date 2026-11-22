NYC Traffic Speed Prediction 
Predictive Modeling 
Chana Neuhaus
DSC 355: Predictive Analytics 
Professor Nasheb Ismaily 





















Introduction and Project Selection
As a commuter who travels daily for work on major interstate highways, I am passionate about initiatives that improve the commuter experience. Anyone who commutes knows the frustration of leaving work on time on a regular Tuesday, only to meet heavy congestion on the turnpike, thanks to rubbernecking fifteen miles down or ill-planned road work in bad weather. Commuters have come to rely heavily on existing solutions, including road signs warning of congestion ahead and the indispensable Waze app, but there is still room for improvement. Specifically, commuters who need to plan their route (to account for gas stops, rest stops, tolls, or coffee pickups along the way) could benefit from a tool that predicts congestion within the next half hour. 
Data
In this project, I use data from NYC DOT speed cameras to build such a model. My project focuses on the outbound Lincoln Tunnel and George Washington Bridge road segments, but could be expanded to include any road with equivalent data. I obtained the data from the NYC data API and filtered for these two road segments. The data contains records approximately every five minutes, with the average vehicle speed over those five minutes. 
Data Wrangling and Feature Engineering
I noticed early on that the dataset contained a large number of observations with a speed of 0 mph. These records pose a challenge: zero mph is perfectly plausible during extreme congestion, but the volume suggests that at least some of these observations are faulty, perhaps due to a camera malfunction or a temporary road closure. It is hard to weed out which instances are real and which are mistakes. As a data-wrangling heuristic, I removed any zero-mph observations that occurred on days with more than 50% of observations being zero mph. While this is not a perfect strategy, it hopefully alleviates some of the errors that would otherwise ensue. 
Using the existing data, I create features for recent speeds (working backward for half an hour), the standard deviation over the half hour, and the recent change over the past 15 minutes. I then define the target: current_speed in the next half hour. I also add a Boolean is_holiday feature. 

Baseline Model
Instead of using a typical dummy regressor, I define the baseline model the way a commuter might. In the absence of other data, the baseline model predicts the target speed to be the current speed. If the average speed on the Lincoln Tunnel is currently 26 mph, we assume it will remain at that rate over the next half hour. I then evaluate this model on the designated test set. The model performs surprisingly well (which is good news for commuters), with MAE of 4.24 and RMSE of 7.33. The model is occasionally prone to making extremely large errors, but otherwise its performance is really good, and, it turns out, hard to beat even with more data. 
Next, I used Statsmodels to build a basic linear regression model, SparkMLlib’s GBTRegressor to build a more sophisticated model, and TensorFlow to build a deep learning neural network model. The linear regression scored an MAE of 4.78 and an RMSE of 7.27, not an improvement over the baseline. SparkMLlib scored an MAE of 4.49 and an RMSE of 6.93. The low RMSE here translates into less extreme errors, which might make it a more reliable choice than the baseline. The TensorFlow model scores an MAE of 4.81 and an RMSE of 7.22, making it an unattractive option. 
. The results of this project offer encouraging advice to commuters: While recent changes in speed, time of day, and year influence congestion levels, the strongest predictor of speed in the next half hour is still the current speed. This offers an efficient, reliable, and cost-effective solution for commuters.
For a more reliable model with a lower likelihood of extreme errors, the Spark GBTRegressor is a decent solution. It could be implemented as a dashboard that displays current speed alongside the model's prediction for speed over the next half hour.
This model could be expanded and trained on more road segments using the same data source. This framework and data could also be used to predict speeds over time intervals, such as a full hour in advance or even a few hours in advance (perhaps a scenario where a predictive model would perform significantly better than our baseline). 

