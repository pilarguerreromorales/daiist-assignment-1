# Assignment 1 Report

- **Name**: Maria del Pilar Guerrero Morales
- **Student ID**: 20373
- **Email**: pguerrero.ieu2022@student.ie.edu
- **Group**: BBADBA 5A 

## Dataset

*What is it, where did you get it, what does one row represent, how many
rows/columns, and why did you pick it.*

I chose the Default of Credit Card Clients dataset from the UCI repository, downloaded through Kaggle. 
The link to the dataset: https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset

In terms of content, in my dataset each row is one credit card holder at a Taiwanese bank, described by their credit limit, a few demographic fields and six months of history that includes: monthly repayment status, statement amount and amount paid. The file has 30,000 rows and 25 columns. I checked and it has no missing values and no duplicate clients. The target is whether the client will default the next month and it is positive for 22.1% of clients. 

I chose it because during this semester I was working in my internship at Visa and some of the projects we handled explore use cases for AI across commercial banks. While under the new EU AI Act credit authorisations might be a high-risk use case for AI, I was very intrigued by the progress being made with foundation models by players like Nubank and Revolut, which are turning commercial banking into an optimisable, cloud-native and personalised experience. At a smaller scale, I thought it would be an interesting topic to investigate on my own through this assignment.

As a flag, although it looks clean, I did notice it has several undocumented codes for example EDUCATION values 0, 5 and 6, and repayment-status values −2 and 0 as well as 1,930 clients with negative bills which I plan to handle in

## Business / real-life framing

I think the data is inherently suited to a model that supports a monthly risk review at the bank. Hypothetically, every existing cardholder is scored, and clients above a chosen threshold have their credit limit lowered to reduce the bank's exposure before a possible default. This way a bank can manage the expected loss from defaults against the expected interest revenue. Because the features need past data, I'd require at least 6 months of payment history, hence the model only applies to existing customers; new applicants would need a separate solution based on a different set of data (e.g. Experian credit scores). In my analysis I will use the target as given in terms of the expected default for the next month (yes or no)

All clients are observed over the same six months (April to September 2005) and each appears only once, so a random split cannot leak future information into training. I therefore plan to use a stratified 70/15/15 split, which keeps the 22% default rate equal across train, validation and test. This is very important because of the class imbalance between default and non-default. The validation set is used to tune regularisation, the learning rate and the threshold, in other words the hyperparameters. The test set will only be used once, for the final comparison. The cost of this choice is that I cannot test whether the model still works in a later period, such as Taiwan's 2005–06 card-debt crisis that followed, because the economy and the situation would be different: there would be data drift. With the available data it is still worth studying common behaviours and indicators of credit risk.

In terms of the types of errors, the two do not cost the same. Missing a defaulter (FN) means losing much of the outstanding balance, while wrongly lowering a good client's limit (FP) costs some lost revenue and maybe a bit of reputation/ customer satisfaction. For an estimate in terms of cost (to be later used in the model) I set FN = 5 and FP = 1. According to my research there is no single industry standard but the German Credit dataset, also from the UCI has an official cost matrix with these weights. An implication of this is that  accuracy is the wrong metric, since predicting "no default" for everyone already scores about 78%. Instead I will choose the threshold that minimises total cost on the validation set. I also plan on reporting ROC-AUC and PR-AUC as further supporting metrics, which do not depend on a threshold.

Finally, to make this hypothetical scenario more realistic I've decided to exclude the SEX and MARRIAGE variables, because credit regulations commonly prohibit using them in lending decisions, and it will help focus on how past repayment patterns actually predict default.

## Data preparation & feature engineering

*What you engineered and why, and any data-quality decisions you made along
the way — e.g. "segment X had defective data, so I excluded it and used a
population-average default for scope Y at inference time; the impact of
that choice is Z."*

## Modeling: three implementations, one model

*Which model (linear or logistic regression) and why. A results table
comparing scikit-learn, the manual PyTorch loop, and the standard
torch.nn.Module/torch.optim workflow, on the same test set, against the
naive baseline. Do the three agree? If not, why not?*

## Limitations & next steps

*Real limitations you found, and concretely how you'd address each one with
more time or data — not generic hedging.*

## Generative AI use disclosure

*Per the syllabus AI Policy: what you used and how, or "no AI content used."*
