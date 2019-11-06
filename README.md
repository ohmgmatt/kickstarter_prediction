# Prediction of Kickstarter Projects

## Background

Kickstarter is a crowdsourcing platform where Creators can upload their project for users to back. Many successful projects have come from this. However, on Kickstarter's [statistics page](https://www.kickstarter.com/help/stats), it shows that projects only have a success rate of 37%. In this project, we'll be determining what features about a project lead to success and creating a model that predicts the outcome of a project. Specifically, the goal is to predict if the project will fail to provide insight to Creators that there needs to be work done on their project.

## Data Source

Kickstarter does not have a public API or an easy way to gather information about the projects. The team at [WebRobots](webrobots.io) have created a dataset of Kickstarter projects using their software found [here](https://webrobots.io/kickstarter-datasets/).

In addition to the dataset from WebRobots, I crafted a web parser that utilized the URLs from the WebRobots dataset and pulled additional data such as descriptions and rewards. I ran the script over several days by utilizing Google Cloud Platform's [Compute Engine](https://cloud.google.com/compute/). The code for the script can be found [here](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/01_parse_urls.py).

## Data Wrangling
Several columns from the project were not helpful in relation to the project and the predictive capabilities. These columns were dropped.

Some projects, at the time of the data pull, were still ongoing and therefore categorized as such in the dataset. Without having complete information on whether the projects succeeded or failed, they were also dropped from the dataset.

From our web parser script, if the a project did not contain enough information or returned errors (i.e. having an inaccessible description), they were dropped from the dataset.

A full breakdown along with an exploratory analysis of the data can be found in the [Milestone Report](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Milestone%20Report/Milestone%20Report.pdf).

## Algorithms and Machine Learning

### Scoring

Our business case is to provide prospective Creators the insight on whether their project will do well or not. We've also defined that we care more about providing "failed" predictions than "successful" predictions since this provides more actionable insight. A Creator is able to confidently re-work their project knowing there is more to do in their campaign.   
Due to this, simply relying on accuracy is not going to be enough. We can to make sure we are recalling all "failed" projects and classifying them as such. In addition, we'd also want to minimize the amount of reworking Creators have to do so we need to minimize the amount of "successful" projects that we classify as "failed". From this, we can utilize the fbeta score. We'd also want to put an emphasis to recall so we'll end up with a beta > 1.0.

### Modeling
[Model Analysis](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Notebooks/Modeling_In-Depth%20Analysis.ipynb)  
[Text Analysis](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Notebooks/Modeling_Predictive%20Words.ipynb)  

The modeling process followed four steps:
- Initial Modeling
- Hyper-parameter Tuning
- Stacking Ensemble
- Thresholding

I first test various models such as Logistic Regression and Linear SVC, utilizing the scikit-learn library, to get a base understanding of how each model performs. I then tune some models with methods such as K-fold cross validation, grid search cross validation, and randomized search cross validations. After, I utilize a stacking ensemble on top of the highest scoring model to improve scores. Finally, I threshold the model to ensure we're improving our model to fit our business case. In this case, we reduced the threshold to below .4 to classify more fails than not. Overall, our model has a fbeta score of 80%.


## Content
[Notebooks](https://github.com/ohmgmatt/kickstarter_prediction/tree/master/Notebooks)  
[Milestone Reports](https://github.com/ohmgmatt/kickstarter_prediction/tree/master/Milestone%20Report)  
[Final Report](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Final%20Report.pdf)
