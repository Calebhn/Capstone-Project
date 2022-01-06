# Starbucks Capstone-Project
## Requirements
### Libraries
* pandas
* numpy
* pickle
* matplotlib.pyplot
* json

### Programs
* Have Jupyter Notbook reader

## Introduction

This data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks. 

Not all users receive the same offer, and that is the challenge to solve with this data set.

Your task is to combine transaction, demographic and offer data to determine which demographic groups respond best to which offer type. This data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.

Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. You'll see in the data set that informational offers have a validity period even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, you can assume the customer is feeling the influence of the offer for 7 days after receiving the advertisement.

You'll be given transactional data showing user purchases made on the app including the timestamp of purchase and the amount of money spent on a purchase. This transactional data also has a record for each offer that a user receives as well as a record for when a user actually views the offer. There are also records for when a user completes an offer. 

Keep in mind as well that someone using the app might make a purchase through the app without having received an offer or seen an offer.

### Example

To give an example, a user could receive a discount offer buy 10 dollars get 2 off on Monday. The offer is valid for 10 days from receipt. If the customer accumulates at least 10 dollars in purchases during the validity period, the customer completes the offer.

However, there are a few things to watch out for in this data set. Customers do not opt into the offers that they receive; in other words, a user can receive an offer, never actually view the offer, and still complete the offer. For example, a user might receive the "buy 10 dollars get 2 dollars off offer", but the user never opens the offer during the 10 day validity period. The customer spends 15 dollars during those ten days. There will be an offer completion record in the data set; however, the customer was not influenced by the offer because the customer never viewed the offer.

## Data Sets

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files:

**portfolio.json**
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

**profile.json**
* age (int) - age of the customer 
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

**transcript.json**
* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

### Cleaning Process
File: ETL Pilepline.ipynb
The .json files given as input for this project have been cleaned and transformed so that the extraction information was easier. 

ETL Pipeline Jupyter Notebook contains all the process to extract and transform the data sets. Some of the cleaning and transformation done to the data has been:
* Transforming date into integer number defining the number of days the customer has been members in the application.
* Divide value column containing dictionary and creating a column for the keys and other for the values of each dict. 
* Filling NaN values in the amount column with 0 meaning that there was no transaction done.
* Creating dummy variables for the event column
* Map customer id to a numeric form (integers going from 1 to 14824). This helped identify them easily when working with data.
* Same mapping was done with the offer id (going from 0 to 9)
* Suspicious customer profiles were deleted from the data sets. Customers with no gender, no income and age of 118 years where deleted.

#### Files created
All files are in the data folder inside this git
* new_profile.json
* portfolio_clean.json
* transcript_profile_merge.json
 
 ## Statistical analysis, Model generation & validation and creation of a Recommendation engine

All the process concerning the analysis and creation of the recommendation engine is defined in the “Starbucks_Offers_Analysis.ipynb” jupyter notebook provided in this git. 
### Recomendation engine
Different type of recomendations have been used for this pproject. 
* Collaborative filtering recomendation is used to find the best offers we can give to each specific customer considering how other customers reacted earlier to this offers. 
* Content based recommentation has been used to reommend offers to new users. 
