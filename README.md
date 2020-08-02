# teemo-tool-predict

This is part of the [Teemo Tool](https://github.com/michaelmdresser/teemo-tool) project.

This is a series of Jupyter notebooks whose goal was to analyze the match data gathered by the project's [Riot API Scraper](https://github.com/michaelmdresser/teemo-tool-data). The basic concept was to build a model based on team composition to predict the outcome of low-rank (roughly sub-Silver) League of Legends matches. While a good goal, it turned out to be a foolish endeavor, as you will see if you inspect the notebooks. Beware that runtime for some operations is long. Requires a SQLite database from the scraper. If you want to do some analysis of your own, reach out to me and I can send you some data.

Future work may be done to investigate individual champion win rates, but I believe there are others tools and/or sites that perform this aggregation.

I recognize that my analytic process was not the most robust, but the lack of an "easy win" or really any indication of predictive ability from the models I tried put me off. 

## Notebooks

#### `build-features.ipynb`

The first attempt at data analysis. Builds a feature vector of length `number_of_champions * 2` where the top half of the vector contains 5 `1`s (the rest `0`) corresponding to the presence of champions on one team. The bottom half represents the same but for the other team. Predicts if team with ID `100` will win. Trained and evaluated against a logistic regression classifier and several neural net classifiers of varying architectures on a dataset of roughly 80000 matches. An SVM classifier was evaluated at one point but took too long to train while producing the same results as the logistic regression. Data was generated from the API scraper running with an MMR threshold of 650.

#### `build-features-filter.ipynb`

Builds on `build-features.ipynb` by adding a data filtering step. The goal of this step was to more stringently enforce an MMR threshold of the players in a game to be evaluated, with the idea that a cleaner dataset in this dimension would provide better results. See the notebook for results, but the summary is the same as `build-features.ipynb`: prediction based on this data is meaningless (as bad as a coin flip). Because filtering take an enormous amount of time, evaluation was only performed against the first 20000 matches in the same dataset. Optimization could be performed with smart joins using SQLite's JSON1 estension but I am not that versed in its magic.

#### `loganalysis.ipynb` and `loganalysis-2.ipynb`

Notebooks to analyze processing speed of the filter step of `build-features-filter.ipynb`. `loganalysis.ipynb` is deprecated due to a change in logging format.
