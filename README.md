# Springboard Capstone 1
## Prediction of Kickstarter Projects

Kickstarter is a crowdsourcing platform where Creators can upload their project for users to back. Many successful projects come from this. However, on Kickstarter's [statistics page](https://www.kickstarter.com/help/stats), it shows that projects only have a success rate of 37%. In this project, we'll be determining what features about a project lead to success and creating a model that predicts if a project will fail. The goal is to provide insight to Creators that there needs to be work done on their project if it is predicted to fail.

## Data Source

Kickstarter does not have a public API or an easy way to gather information about projects. The team at [WebRobots](webrobots.io) have created a dataset of Kickstarter projects using their software found [here](https://webrobots.io/kickstarter-datasets/)

In addition to the dataset from WebRobots, I crafted a web parser that utilized the URLs from the WebRobots dataset and pulled additional data such as descriptions and rewards. I ran the script over several days utilizing Google Cloud Platform's [Compute Engine](https://cloud.google.com/compute/). The code for the script can be found [here](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/01_parse_urls.py)

## Data Wrangling
Several columns from the project were not helpful in relation to the project and the predicting capabilities. These columns were dropped.

Some projects, at the time of the data pull, were still ongoing and therefore categorized as such in the dataset. Without having complete information on whether the projects succeeded or failed, they were also dropped from the dataset.

As a result from the parsing, if a project did not contain enough information or returned errors (i.e. having an inaccessible description), they were dropped from dataset.

A full breakdown along with an exploratory analysis of the data can be found in the [Milestone Report](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Milestone%20Report/Milestone%20Report.pdf)

## Algorithms & Machine Learning

### Scoring

To align with the business case of presenting Creators with information regarding their project, I'd like to track failures more than successes. Because of this, there is an emphasis on the recall of the project and I'll be using an f-beta scorer with a beta > 1.

### Modeling
[Model Analysis](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Notebooks/Modeling_In-Depth%20Analysis.ipynb)  
[Text Analysis](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Notebooks/Modeling_Predictive%20Words.ipynb)  

I test various models like Logistic Regression and Linear SVC from the scikit-learn library to get a base understanding of how each model performs. I then tune some models through grid searching and randomized searching to find the optimal parameters. Through this, a tuned Random Forest Classifier is the final model.

To improve the score, I employ a stacking ensemble where I utilize a Multinomial Naive Bayes classifier on the text data and take the labels as features for the Random Forest classifier. This method improves the scoring of the algorithm.


## Content
[Notebooks](https://github.com/ohmgmatt/kickstarter_prediction/tree/master/Notebooks)  
[Milestone Reports](https://github.com/ohmgmatt/kickstarter_prediction/tree/master/Milestone%20Report)  
[Final Report](https://github.com/ohmgmatt/kickstarter_prediction/blob/master/Final%20Report.pdf)
