# NPS Aggregated model for targets, simulations and explainability.

In this repository, you'll find a curated collection of Jupyter notebooks, each serving as a vital component in the comprehensive framework designed for predicting, simulating, and providing insights into the Net Promoter Score (NPS) at Iberia. These notebooks encapsulate a series of algorithms and functions meticulously developed to handle various aspects of the NPS analysis pipeline.

From preprocessing raw data to applying sophisticated predictive models, and from running detailed simulations to unraveling the underlying factors influencing customer satisfaction, the contents of this repository are instrumental in steering NPS-related strategic decisions. The assortment of notebooks not only facilitates accurate NPS forecasting but also demystifies the intricacies behind the scores, offering a transparent and interpretable view of the results.

By leveraging the interactive nature of Jupyter notebooks, the repository enables a dynamic environment where stakeholders can observe the effects of different parameters in real-time, thereby fostering an informed and data-driven culture within the organization. This repository is the cornerstone of a robust analytics platform that empowers Iberia to harness the full potential of NPS metrics, ensuring that customer feedback translates into meaningful action and sustained improvement.

## Production Notebooks Section
The distilled essence of the research conducted in the Development Notebooks is presented here, divided into three distinct stages:

1. Read and Aggregation Notebooks
This stage includes four structured notebooks, each responsible for:

Extracting data from the latest s3 bucket containing nps_historic and kpis tables.
Aggregating data to derive key variables, primarily focusing on touchpoint satisfactions and Net Promoter Scores (NPS).
Saving the aggregated dataframes as .csv files.
While initial experiments utilized weekly aggregation, the production phase primarily relied on the output from NPS_adjustment_daily. The notebooks NPS_adjustment_monthly and NPS_adjustment_yearly are utilized for providing SHAP value comparisons across different time aggregations. These notebooks also serve as a foundation for estimating targets for the following year, using either autoregressive methods or manual estimations.

An additional "eda" notebook is included, where the time dependencies of the variables were analyzed.

2. Targets Estimation Notebooks
Currently, this stage contains a single "Targets" notebook. This notebook was instrumental in manually estimating the targets for the upcoming year (2024). It draws data from the monthly and yearly adjustments and generates the final dataframe for model predictions and SHAP value comparisons.

3. Aggregated Model Notebooks
This final stage consists of two notebooks:

a. Retraining Notebook: Utilizes daily aggregated data for model retraining.
b. Prediction Notebook: Employs the trained model and target outputs for making predictions.
The model and its associated components, including scalers, dataframes, and more, are stored within the OPTUNA_EXP directory, specifically in All_var_all_tp_best_models. The All_var_all_tp directory contains various reports and graphics related to each cabin/haul.


## Development Notebooks Section
This section encompasses a series of notebooks dedicated to time-dependent experiments. The primary focus of these experiments is to develop a Regression-like model that maintains its explainability. For this purpose, a methodology centered around lagged variable time series was employed. The Darts library was selected for its native support of Optuna and Darts, along with compatibility with most sklearn regression models.
![Project evolution](project_evolution.png)

1. Adjusted vs raw. 
The performance comparison between using raw NPS data and weighted (and adjusted) data showed similar results. However, in terms of Economy cabins, which hold significant weight in the overall percentages, there was better performance observed with the adjusted data. Regarding the model trained with daily aggregated data, this experiment has not been conducted yet and should be considered in future iterations.

RAW monthly forecasts:
![Monthly forecast RAW NPS plot](NPS_raw_forecast.png)
![Monthly forecast RAW NPS mae](NPS_raw_mae.png)
Ajusted monthly forecasts:
![Monthly forecast adjusted NPS plot](NPS_adjusted_forecast.png)
![Monthly forecast adjusted NPS mae](NPS_adjusted_mae.png)

2. Time lags monthly aggregation. 
In the previous plots, it was ploted that forecasts extended six months into the future. To enhance model generalization, backtests for each model variant were conducted.

![Monthly backtest adjusted NPS](NPS_adjusted_backtest.png)

Despite tuning efforts, including the use of Optuna, the performance of the models remained suboptimal, particularly for less populated cabin categories. A notable challenge encountered was in the realm of explainability. Due to high correlation among variables at this level of aggregation, SHAP values did not align with expectations.
![Correlation comparison](correlation_comparison.png)

3. Absence of lags. Daily vs weekly training. Monotonic constraints.






4. Aggregated model vs individual model. Simulation issue.
![Model Comparison with real 2023 data](model_comparison.png)