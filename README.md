<h1>Predicting FIFA Rating Changes Premier League Soccer Player Injuries</h1>

<h2>Introduction</h2>

<p>Injuries are a pivotal factor in the trajectory of a professional soccer player's career, yet decisions around player development, selection, or trade are often made with limited empirical evidence on how injuries affect long-term performance. Our project seeks to address this gap by building a predictive model that estimates the change in a player’s FIFA rating based on their age, number of injuries, and the severity of those injuries. FIFA ratings are widely used as standardized performance metrics by clubs, scouts, and analysts, making them a relevant and accessible proxy for assessing the career impact of injuries.

Our primary stakeholders include soccer clubs, coaching staff, trainers, and sports analysts, who are individuals that can benefit from evidence-based tools to inform decisions related to injured players. By providing a model that predicts whether a player’s rating is likely to increase, decrease, or remain stable following injury, our work offers a data-driven layer of insight into post-injury performance recovery.

With Jade Matrone, Shivani Pandeti, and Lauren Gracias, we approached this need by training machine learning models on a dataset that combines detailed injury histories with official FIFA ratings from the most recent seasons. While our solution is not exhaustive, due to limitations in historical data availability and the subjective nature of FIFA scores, it represents a meaningful step toward quantifying the impact of injuries on player value and developing more informed, data-supported approaches to player evaluation.</p>

<h2>Literature Review</h2>

<p>The impact of injuries in professional soccer has been widely studied with a focus on outcomes like physical performance decline, team success, recovery time, methods of injury prevention, and financial costs. Many studies utilized a combination of statistical methods and machine learning algorithms, including regression models, decision trees, support vector machines, and forecasting techniques. Some research on the A-League and Spanish LaLiga™ found that injuries negatively affect goal difference, match points, and player speed (Lu & Raya-González). In addition, a broader study on European soccer teams showed a direct link between injury burden and team success, with lower injury rates correlating with better league rankings (Hägglund). Other studies also focused on the injuries of those in certain positions, while a more data-driven approach applied GPS tracking and machine learning to forecast injuries. This study demonstrated improved accuracy and provided some actionable insights for injury prevention (Rossi). While previous studies provide valuable insights into short-term injury effects, they often fail to consider long-term player development and perceived performance value.

Specifically, little research has examined how injuries affect players over time, especially through a lens of standardized performance metrics. FIFA ratings offer a consistent metric for player value but are underutilized in academic research. One study did attempt to analyze player performance changes, but did not define key players or use FIFA scores as a consistent metric (Szczecinski). In our proposal, we initially considered defining key players by their FIFA scores, but ultimately shifted focus toward predicting the rating changes based on injury frequency, severity, and age. This pivot emphasizes player trajectories and helps to bridge the gap between existing injury analytics and real-world player evaluation tools used in the soccer industry.</p>

<h2>Data</h2>

<p>Our dataset merges multiple open and scraped sources. Injury and player metadata were sourced from a GitHub repository containing scraped records from Transfermarkt and FBREF, including data on injury type, duration, position, and club affiliation. FIFA rating data for FC24 and FC25 was manually scraped from Fifaratings.com due to the lack of accessible APIs or bulk download options.

<ul>
  <li><a href="data">All Datasets</a></li>
  <li><a href="code_files/fifa_web_scraper.ipynb">FIFA Web Scraper</a></li>
  <li><a href="https://github.com/pkardjian/soccer_injury_risk_prediction/blob/main/README.md">Sourced GitHub Repository</a></li>
  <li><a href="https://www.fifaratings.com/">Fifaratings.com</a></li>
</ul>
  
We focused exclusively on players from top European leagues and applied a minimum FIFA base rating threshold of 75, ensuring our analysis centered on top-tier players most relevant to club decision-making. Incomplete records were removed, and we ensured a mix of injured and non-injured players for comparative modeling. After extensive cleaning and integration—including manual standardization of player names and calculating age from birth year—the final dataset includes key features such as total days injured, injury types, position, age, and the change in FIFA rating between 2024 and 2025 editions.

In addition to injury and demographic information, we incorporated performance metrics such as average number of passes, goals, and touches per game to strengthen the model’s predictive power. Visualizations in the accompanying notebook—including heatmaps, bar charts, and boxplots—illustrate the distribution of injury types, player characteristics, and rating change trends post-injury.
</p>

<h2>Methods</h2>

<p>We framed the problem as both a regression task (predicting continuous FIFA rating changes) and a classification task (predicting the direction of rating change: increase or decrease). Key preprocessing steps included expanding injury records into individual columns by injury type, calculating a total days injured metric, and converting categorical variables such as position into dummy variables.

For regression, we tested Linear Regression, Decision Tree Regressor, Random Forest Regressor, and Gradient Boosting Regressor. For classification, we evaluated Decision Tree and Gradient Boosting classifiers. Hyperparameter tuning and validation strategies (including train-test splits and cross-validation) were used to assess model performance. While the Decision Tree Classifier achieved strong results in predicting rating direction, we ultimately chose to focus on the regression approach. This decision was made to allow for a more nuanced understanding of how individual features—such as age, injury severity, and performance metrics—contribute to the magnitude of rating change.

Among the regression models, the Random Forest Regressor delivered the best performance, though we remained mindful of overfitting concerns and conducted further evaluation to ensure model generalization. Model performance was documented using metrics such as R², accuracy (for classification comparison), and confusion matrices where applicable.

<ul>
  <li><a href="code_files/methods_part1.ipynb">Methods Part 1 Code</a></li>
</ul>

Following our initial analysis (from our initial part 1 results - methods part 1), we continued to apply predictive modeling to explore how injury characteristics relate to changes in player performance, using both regression and classification approaches. We began by treating the task as a regression problem, testing several models including Linear Regression, Decision Tree Regressor, Random Forest Regressor, and Gradient Boosting Regressor. For this analysis, our target variable was rating_change representing the difference between pre- and post-injury performance ratings. Prior to modeling, we excluded identifiers (e.g., player name, team) and redundant or post-injury-dependent features to prevent data leakage. We evaluated models using standard regression metrics—R², Mean Absolute Error (MAE), and Root Mean Squared Error (RMSE)—on an 80/20 train-test split.

We then reformulated the problem as a classification task by categorizing rating_change into three discrete outcomes: increase, decrease, or no change. A Random Forest Classifier was applied, and to better understand model decision-making, we incorporated SHAP to interpret individual prediction contributions. To implement SHAP, we first created a synthetic player by averaging the test set features and adjusting specific variables (like assigning an injury type or age) to simulate different injury scenarios. Using this synthetic player, we predicted the class of performance change using the trained Random Forest model. Next, we applied SHAP’s TreeExplainer to calculate Shapley values, which measure the contribution of each feature to the prediction for that synthetic player. The Shapley values were then visualized using a force plot, which provided a clear illustration of how each feature influenced the predicted class of performance change.

To further evaluate the potential for predicting specific changes in ratings, we also framed the task as a multi-class classification problem. In this setup, each class represented an exact change in player rating. We tested both Decision Tree and Gradient Boosting classifiers, including versions with increased tree depth to handle potential feature interactions and non-linearities. Due to the granularity of this approach, special attention was given to handling class imbalance and evaluating model generalization.</p>

<ul>
  <li><a href="code_files/methods_part2.ipynb">Methods Part 2 Code</a></li>
</ul>

<h2>Results</h2>

<p>We began by exploring regression and classification approaches using the original dataset, which combined player performance statistics and injury history. This dataset included key features such as age, birth year, total days injured, and FIFA ratings from FC 24 and FC 25 editions. The target variable was defined as the difference in FIFA ratings between FC 24 and FC 25, representing the rating_change. In this original setup, we applied several regression models to predict rating change as a continuous outcome. The Random Forest Regressor achieved an R² score of 0.8281727549884175, and the Gradient Boosting Regressor performed even better with an R² score of 0.9043430456294668. In contrast, a basic Linear Regression model had a much lower R² of 0.07528541507215347, confirming that tree-based methods were better suited to this feature space. These strong results suggested potential predictive value, although the feature engineering was still limited in scope at this stage.

To further refine model performance and improve generalization, we created a second version of the dataset with more granular features. This included expanded injury categories, in-game performance metrics, and one-hot encoding for categorical variables like player position and foot preference. While this enhanced feature richness, it increased the total number of variables to over 50 without adding more player records, leading to challenges with overfitting and weak correlations.

For regression, we tested Linear Regression, Random Forest, Gradient Boosting, and Decision Tree models. Linear Regression achieved an R² score of -0.1176, MAE of 1.2915, and RMSE of 1.7035. Random Forest had an R² of -0.1998, MAE of 1.3596, and RMSE of 1.7651. Gradient Boosting resulted in an R² of -0.2427, MAE of 1.3758, and RMSE of 1.7964. The Decision Tree performed the worst, with an R² of -1.0003, MAE of 1.7500, and RMSE of 2.2791. A simple baseline model that always predicted the mean had an MAE of 1.1801 and RMSE of 1.6115. None of the transformed models outperformed the baseline, highlighting the difficulty in learning reliable patterns from the expanded feature set.

For classification, we used two approaches. In the first, we grouped player outcomes into three categories: increase, decrease, and no_change. A Random Forest Classifier trained on this setup achieved an overall accuracy of 0.39. The F1-scores for the three classes were 0.40 for decrease, 0.41 for increase, and 0.37 for no_change. The confusion matrix showed that the model struggled to distinguish between small changes, with many predictions overlapping across categories.

In the second classification setup, we used the actual numeric rating change values (e.g., -5.0 to 6.0) as labels in a multi-class setting. This approach suffered from class imbalance, with several labels having very few or zero correct predictions. The Decision Tree Classifier in this setup achieved an accuracy of 0.2222222222222222 and a weighted F1-score of 0.22. The Gradient Boosting Classifier achieved an accuracy of 0.28125 and a weighted F1-score of 0.22. Both models returned undefined precision and recall for multiple classes due to lack of predictions, and overall performance was only slightly better than random guessing.

While early regression results on the original dataset showed strong performance, the expanded and transformed dataset revealed limitations related to data size, overfitting, and weak feature-target relationships. More data and better feature refinement will be necessary for stable, generalizable predictions.</p>

<h2>Discussion</h2>

<p>Our goal was to build a model that predicts FIFA rating changes after injury to support decisions made by coaches, analysts, and recruitment staff. While we achieved strong results on the original dataset, especially using tree-based regression models, performance dropped significantly after transformation due to limited data and increased feature complexity. Classification models provided some directional insights but lacked consistency.

Although our models didn’t fully meet the goal of accurate post-injury prediction, the work still offers useful signals about which injury factors matter most. To better serve stakeholder needs, future work should focus on increasing dataset size, improving class balance, and incorporating richer contextual or recovery-based features. Collaborating with clubs to access detailed recovery data - such as rehabilitation duration, training intensity, and match fitness - could enhance model precision. In addition, incorporating qualitative information like medical assessments or expert ratings may help bridge the gap between statistical predictions and real-world performance outcomes. With these improvements, the model could become a more actionable tool to support transfer decisions, training plans, and injury management strategies.</p>

<h2>Limitations</h2>

<p>While our project uses machine learning to better understand how injuries affect player performance, several limitations constrain the depth and generalizability of our findings. First, our analysis was restricted by limited historical data availability. We were confined to only the most recent FIFA scores in order to analyze the change from 2024 to 2025, which constrains our ability to model long-term trends or delayed performance effects. A broader time range would have helped us capture cumulative injury impacts and better reflect real-world player development arcs. Also, the subjective nature of the FIFA scores introduced a degree of noise into our target variable. Although FIFA ratings are widely recognized in the soccer industry, they are not based on strictly objective or completely transparent performance algorithms, which may skew some results.

In addition, our data integration process required extensive manual cleaning, like standardizing player names and handling missing values due to scraping issues. These preprocessing steps were necessary for our project, but did open doors for more inconsistency and may have led to the exclusion of valid cases. Also, we did include injury type and duration as indicators for severity, but we did lack contextual factors like match load or training intensity. This limits our ability to differentiate between injuries with similar durations but vastly different implications for performance and recovery.

Another key limitation arose from our expanded dataset, which included more granular features such as one-hot encoding for player position and foot preference. While this enriched the data, it also increased the number of features to over 50, contributing to overfitting and weak correlations in the models. This may have masked some true patterns in the data, as the models struggled to generalize well to new, unseen data. Dimensionality issues in the enriched dataset may have also added complexity without yielding clear improvements in prediction.

In addition, stakeholder needs were only partially addressed. While we focused on injury impacts on player ratings, we did not include match importance or player role as separate factors, which could have influenced the injury recovery process and player performance post-injury. These factors are crucial for clubs when evaluating injury impacts on a player's long-term value to the team. Lastly, despite using SHAP for model interpretability, we recognize that our analysis was still limited in how deeply we could interpret complex injury-performance relationships. While SHAP helped identify key drivers such as injury type and severity, understanding the nuanced interaction of multiple injury events, player recovery time, and match circumstances remains a challenging task. Further exploration with more data and improved feature engineering could potentially offer deeper insights into these relationships.</p>

<h2>Future Work</h2>

<p>We feel that our project could be improved and extended in many different ways. One of the most obvious steps we would like to take next would be to incorporate more FIFA rating data from multiple seasons so that we can better capture long-term trends. By including more seasons or even other leagues we could enhance the model robustness and generalizability. While our model focuses on rating changes as the primary outcome, future work could explore alternative or complementary outcome variables, such as changes in market value, number of matches played post-injury, or subsequent transfer activity. These indicators may offer different perspectives on a player’s perceived value and career trajectory after injury. In order to expand on our model, we would also like to explore other interactions like between injury timing and age. This would help us to see whether younger players recover their ratings more effectively than older players after similar injuries. Including team-level context like minutes played, club success, or squad rotation, could also provide additional predictive power.

Also, future iterations of our project could involve stakeholder feedback in order to evaluate how predictive tools like ours could be embedded into decision-making workflows. Incorporating real-world decision criteria could make the model more actionable in professional settings.</p>

<h2>Citations</h2>

<p>Hägglund, Martin, et al. “Injuries Affect Team Performance Negatively in Professional Football: An 11-Year Follow-up of the UEFA Champions League Injury Study.” British Journal of Sports Medicine, BMJ Publishing Group Ltd and British Association of Sport and Exercise Medicine, 1 Aug. 2013, bjsm.bmj.com/content/47/12/738.short.

Lu, Donna, et al. “The Financial and Performance Cost of Injuries to Teams in Australian Professional Soccer.” Journal of Science and Medicine in Sport, Elsevier, 26 Nov. 2020, www.sciencedirect.com/science/article/abs/pii/S1440244020308136.

Raya-González, J., Pulido, J.J., Beato, M. et al. Analysis of the Effect of Injuries on Match Performance Variables in Professional Soccer Players: A Retrospective, Experimental Longitudinal Design. Sports Med - Open 8, 31 (2022). https://doi.org/10.1186/s40798-022-00427-w.

Rossi, Alessio, et al. “Effective Injury Forecasting in Soccer with GPS Training Data and Machine Learning.” PLOS ONE, Public Library of Science, 25 July 2018, journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0201264#sec003.

Szczecinski, Leszek and Roatis, Iris-Ioana. ‘FIFA Ranking: Evaluation and Path Forward’. 1 Jan. 2022 : 231 – 250.</p>
