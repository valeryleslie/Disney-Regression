# Disney-Regression
A predictive linear regression model to predict the IMDB &amp; TMDB popularity ratings for Disney TV shows &amp; movies using Kaggle dataset at https://www.kaggle.com/datasets/victorsoeiro/ disney-tv-shows-and-movies.

Based on our needs, we subset by columns and perform regression onto:
release_year: the year in which the movie was first released to the public
imdb_score: The rating of a movie listed on IMDB
imdb_votes: The number of reviews done on IMDB for a movie
tmdb_popularity: The popularity score of a movie calculated by TMDB 
tmdb_score: The rating of a movie calculated by TMDB

We found the following:

Multivariate Model to predict IMDB and TMDB scores
	After seeing all the models individually against IMDB and TMDB we decided to perform a multivariate linear regression model fit onto the TMDB and IMDB scores with all the variables we juxtaposed against it before as well as the scores onto each other’s models respectively. 
  Unfortunately, we still get low results for the Adjusted R-squared about 0.5 for each model. To further investigate this result and perhaps make a better model, we decided to perform a BoxCox analysis of each variable to recommend a transform. 

<img width="688" alt="Screen Shot 2022-08-09 at 13 14 10" src="https://user-images.githubusercontent.com/99020772/183752015-77ce8eb1-ffec-4862-a28f-4846ae553aa8.png">

Following the BoxCox results, we decide to only follow the lambda power transform recommendations for IMDB and TMDB scores (ones with a good bell curve shape). This gives us the following model:
imdb_score2 ~ release_year + tmdb_popularity + tmdb_score1.6 + imdb_votes
Intercept: 222.074460; Slope of release_year: -0.109747; Slope of tmdb_popularity: -0.002909; Slope of tmdb_score1.6: 1.829460; Slope of imdb_votes: 0.000017; Adjusted R-squared: 0.590
tmdb_score1.6  ~ release_year + tmdb_popularity + imdb_score2 + imdb_votes
Intercept: -79.076430; Slope of release_year: 0.044603; Slope of tmdb_popularity: 0.001589; Slope of imdb_score2: 0.255879; Slope of imdb_votes: -0.000001; Adjusted R-squared: 0.531
While our Adjusted R-squared may still be unideal, we have improved on our model. To further investigate, we looked into our residual plots. Each predictor variable shows good results–the existence of many outliers explains to us a lower R-squared. Other than that, they are overall significant with a 5% significance level save for the TMDB Popularity Score in the IMDB score model. Below are the residual plots:
<img width="1084" alt="Screen Shot 2022-08-09 at 13 11 54" src="https://user-images.githubusercontent.com/99020772/183752203-2e1b4855-b465-4fc7-9db2-ed2fd0c251ef.png">

Diagnostics for our IMDB model:
<img width="870" alt="Screen Shot 2022-08-09 at 13 12 29" src="https://user-images.githubusercontent.com/99020772/183752141-45cf7ff0-e866-4859-b88f-aa0b65d0d487.png">

Diagnostics for out TMDB model:
<img width="870" alt="Screen Shot 2022-08-09 at 13 12 29" src="https://user-images.githubusercontent.com/99020772/183752312-6bd04602-cc7e-413d-a4f7-bffc7df3eee9.png">

By the diagnostic plots, we could see that this model may have problems with normality by the tail deviations with the tail parts of the QQ plot but overall both models have a good hold over the constant variance condition. The lack of hold of normality is expected with the large deviations of the data. However, despite the lack of normality, the fact that we have a large sample size, we can still get reasonable values in prediction and trust the confidence intervals made and the t-test values from the summary of the models. Lastly, the lack of bad leverage points from the residual vs leverage plot shows good promise in the model’s validity. 
Discussion and Conclusion
Overall, we conclude that with the data in the Disney+ dataset, we may not be able to accurately predict the IMDB and TMDB scores. There are simply too many extreme variables of large ranges; namely TMDB popularity scores that do contribute to the model but do not help to be able to take such a large range of values. However, our final model is still promising in some way to predicting the data; indicated from our diagnostic and residual plot, a large chunk of the data is still able to be predicted quite accurately. Hence we present our multivariate linear regression model to make a rough prediction for an IMDB and TMDB score is the following: 
imdb_score2 = 222.074460 - 0.109747*release_year - 0.002909*tmdb_popularity + 
1.829460*tmdb_score1.6 + 0.000017* imdb_votes
tmdb_score1.6  = -79.076430 + 0.044603*release_year + 0.001589*tmdb_popularity + 
0.255879*imdb_score2 - 0.000001*imdb_votes
	Last but not least, seeing the sudden jump for <0.1 Adjusted R-squared from the other predictors up to the final model making a jump to 0.5 Adjusted R-squared, we conclude that the IMDB and TMDB scores are strongly linearly correlated with one another–and having the other would help greatly in predicting the scoring of a movie for the other website. 
