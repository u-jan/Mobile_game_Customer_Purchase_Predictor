PROJECT: Profit maximization for a pay to win mobile game.

Simultaneously perusing readme with the notebooks is recommended :)



--- UJ Project Proposal --- -------------------------------------------------------------------------

- Revenue Model Optimization -
  The company I am willing to work with for my capstone project is a
mobile game development company which delivers its games through IOS and Android app stores.

My plan is to maximize the revenue generation of a game produced by this company(let's call it
game Alpha)

Game Alpha has two major sources of revenue which are an in-game advertisement
and
pay to win dynamics(pay to proceed failure death in the game).

Revenue Model:
  All of the company's games are free to play and levels are initially less
challenging compared to later levels. As players go through the levels they
begin to fail due to an incremental increase in the challenging nature of the levels.

Following the failure, players have 4 options;
1) Start over  and risk all the progress
2) in-game currency (let's call these coins) which can be bought or added to
players account as an effort of the promotional campaign.
  2.1) if current_coin_count < demanded_coin_count_to_continue:
          buy coins with real currency
       if current_coin_count >= demanded_coin_count_to_continue:
          buy your right to continue in expense of coins
3) Watch advertisement which costs time to players
4) Stop playing

There is an additional critical piece of information about this revenue model.
Neither players nor the developers do not know the exact occasion players will
be offered to spend coin or to watch adds because there is an algorithm which
randomizes the offer decision based on some logical reasoning.

With consideration of the revenue model of the company, I plan to
create classification models in order to predict if a customer will
(1) use in-game currency
or
(2) watch ads

My ultimate goal within this capstone project is to use such findings
to present data-driven optimization recommendations which are expected to
increase the revenue of the company. Within the information in hand and my degree of
involvement with the company as well as the data provided by the company, I
expect my recommendations to shape
(1)product design and gaming experience,
(2)pay to win decision algorithm and,
(3)promotional campaigns.

Strategic Note: As I did not start working on the project yet, I am not sure about how to increase revenue.
I hope to find a way as I go through EDA. However, I have a guess about where to concentrate my efforts.
Using in-game currency generates significantly more revenue compared to watching ads. Moreover, the
offers players receive includes adwatch offer by random chance. I believe if I can optimize the adwatch delivery
percentage and target I can maximize the revenue. We will see. 

-- My professional and personal motivation(2018) -- -------------------------------------------------------------------------

    Considering my marketing and start-up growth-oriented business background, I
believe working on this capstone project will be highly beneficial for my
technical improvement as well as career development because I will be able to
use a variety of significant data science tools in order to come up with business
solutions which are applicable in various industries. In addition to the project's
wide range of applicability among a number of industries, mentioned revenue
model is trending among leading gaming companies. Therefore even after the
'project' is completed I plan to increase its complexity and improve its
effectiveness as I perceive it as a benchmark model to optimize the revenue model
of industry leading firms.
    I am interested in this project not only professionally
but also personally because the driving force which introduced me to data science
is to better understand consumer behavior and the factors that influence
purchasing decisions. Thus this is a golden opportunity for me to use industry
best practices, and leading-edge technologies to predict customer churn, optimize
product pricing, and create value.
    Finally, I am highly interested in the gaming industry which is extending
its reach 11% annually as it serves the society by entertaining and shaping
different demographics.

-- Data --(feel free to see Appendix)

As the analyst please feel free to find a sample piece of data collected.
Dataset 1: SaveMe --> Includes data about the players when save option is offered
to the player upon failure in a level.

the features of the player at the time an offer is given.
Data Note: The data is in json format.



--------------------------------------------------------------------------------------------------------------------------------------------------
Progress Notes
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Step_1_Open_Them_Up:
- Opened Json files, and saved them as csv files.
- Ran an analisis on dataset in order to highlight expectations
about data preparation.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Step_2_data_engine_part_1:
- Some basic EDA, gathered notes and links about data preparation.
- Some features at this point accounted for string format
dictionaries. Found ways to include them in the model as basic features.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Step_2_data_engine_part_2:
- Actually worked on data preparation. 
- Generated functions in order to create statistically processable data frames.
note: Came back to this point after completing step_4_mvp because 
had problems with getting signal for positive predictions. 

Improvement Point: Check if creating dummy variables for
countries with higher disposable income as I simply group
the rest of the countries together, increases model performance
or not.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Step_3_Basic_Model:
- target: adwatch and coin_spending combined.
- completed data preparation for 3 features
- created a logistic dummy model

             precision    recall  f1-score   support

      False       0.86      1.00      0.93    276578
       True       0.00      0.00      0.00     43401

avg / total       0.75      0.86      0.80    319979

- Conclusion: due to unbalanced data which led to lack of
signal for true predictions the model predicted that every player 
will refuse both offers. Accuracy score was high as a result of
unbalanced data again. 

As a solution I decided to use SMOTE to oversample for next steps.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Step_4_MVP:
- SMOTE on 10k dataframe.
- GB model

              precision    recall  f1-score   support

       False       0.89      0.97      0.92      2571
        True       0.50      0.22      0.30  
        
- Recursive Feature Elimination for logistic Model(on oversampled data)
- Logistic Regression(get rid of high p-value features)


               precision    recall  f1-score   support

      False       0.67      0.85      0.75      1759
       True       0.81      0.60      0.69      1855

avg / total       0.74      0.72      0.72      3614

- adOption is included so this model is useless. --> mistakes were made.

Scores look fine for mvp. But at this point I understood that I made
a incredible mistake. Although my target was a combination of adwatch 
and coin_spending, the features included adOption. However adwatch can not be
True if adOption is False. In other words adwatch 100% depended on
adOption which generated strong signal but did not really teach us anything.

From this point on I decided to seperate targets which led to seperate analisis on
adwatch, and coin spending as called target_adwatch and target_bought.
Although I included adOption feature within the analisis on target_bought,
I droped the feature as I run analisis on target_adwatch.

Also understood that I need to go deeper within EDA. 


-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Simply sharing my honest thoughts about the project.

As completing step 4 I have a couple of problems.
- The combined target does not receive enough signal to make my model predict positive with any given data. 
- I have no idea about the ways to add business measurable business to the company with this project.

Assumption --> If I start seperating the target and splitting the data based on particular given conditions
I may receieve signal and understand how to make more money.

Solution: Split the data frame on given conditions such as:

CONDITIONAL PROB 1: if the adOption is provided to the player or not
CONDITIONAL PROB 2: if the player has enough coins as offer is given or not
CONDITIONAL PROB 3: if the player spent actual money on coins before or not

and check if the there is a meaningful difference for target: adwatch and target:spend coin.
------------------------------------------------------------------------------------------------------------------------------------------------------

EDA on conditional probability 2 and 3 did not lead to any behavioral difference when it comes to
adwatch and coin spending patterns.

On the other hand;
CONDITIONAL PROB 1 EDA findings: In accordance to historical data from almost a million data points -

Probability of adOption == 1: 21% 
Probability of adOption == 0: 79% 

Given adoption == 1 probability of a player to use coins 3.3%
Given adoption == 0 probability of a player to use coins 7.8%



Interesting, In accordance to historical data when players do not receive the adoption
their probability to buy coins is significantly higher(proven with A/B testing as well.)

Certainly completely eliminating adOption will not be benefitial because most
players are expected to be conservative in terms of their disposable income
when it comes to free to play games-applications.

However if I find a way to target the right players to show adOption I can increase the
revenue for this company.

Strategy In accordance to conditional prob 1:
(1) If the model predicts that the player is not going to spend coins --> give them adOption
so the company can still generate revenue from players which demonstrate lower tendency to
spend actual money.

(2) If the model predicts that the player is going to spend coins --> do not! give them adOption
because when adOption is given players are less likely to spend coins which generates much more revenue
compared to adwatch.

Questions to Answer;

WHAT IF, we set adOption == 1 for 100% of the offers? 
--> we lose leverage to make players spend actual currency.
--> I also want to add a point in with respect to my domain expertise as an old time competative gamer
within my own behaviors and my gamer network's behavioral patterns I noticed that when people invest not only
time but also money in a game, they become more addictive due to higher stakes of investment. 

WHAT IF, we set adOption == 0 for 100% of the offers? 
--> if the player is not already motivated to spend money on the
game, we eliminate any chances of making some money from this player. 


------- CONDITIONAL PROB 1: dsplit_adoption.ipython (further analisis) ------------------------------------------------------------------------------

Corrolation Heatmap Results:
In accordance to heatmap, target_bought(coin_spending) is 
positively corrolated with: causal_mode , 4, hasEnoughCoin, playTime, level number
negatively corrolated with: adOption (as we already knew)

- causal_mode --> players which prefer causal mode are more likely to use coins compared to 
alternative modes(this is probablydue to that the causal mode is more competative in world ranking).

- 4 --> The number of coins to continue can be 4,5 or 6. Since 4 is the least number of coins asked
in an offer, players are more likely to accept the coin spending offer. In other words players are sensitive
about the amount of coins asked.

- hasEnoughCoin  --> if a player has enough coin in their bank they are more likely
to spend those at the moment of offer(I am not surprised at all).

- playTime --> more time a player spends on a particular level,
more likely they are to commit coins because such behavior is a sign of struggle. 
More frustrated they become, more leverage they have to spend money. 

- level number --> higher level numbers lead to increased coin spending. 
Again makes sense due to the fact that 
(1)upper levels are more challenging
(2)Players which achive upper levels are likely to be more committed to the game. 

and as already target bought is  negatively corrolated with
- adOption  --> not surprised because if adOption is provided players are more
likely to watch adds in order to save coins


---Modelling---

Logistic Regression Classification Report


              precision    recall  f1-score   support

       False       0.99      0.55      0.71    269089
        True       0.14      0.94      0.24     19989

   micro avg       0.58      0.58      0.58    289078
   macro avg       0.56      0.75      0.47    289078
weighted avg       0.93      0.58      0.68    289078



Looks as if the logistic model performs worse compared to initial ones eh? 
Just by checking the accuracy score you are most certainly right. 
Yet as a reminder, the former models scored high on accuracy simply by setting
all the prediction outputs to Negative(false). 
On the other hand this one actually did some 
True predictions which led to a more balanced scores for positive 
and negative predictions.

Gradient Boosts Classification Report


              precision    recall  f1-score   support

       False       0.93      1.00      0.96     26929
        True       0.46      0.01      0.03      1979

   micro avg       0.93      0.93      0.93     28908
   macro avg       0.70      0.51      0.50     28908
weighted avg       0.90      0.93      0.90     28908


Although accuracy scores of both models are same, 
only 1% recall score is concerning me with the gradient boost classifier.
Thereby I will go with the logistic regression model.

-- ROC Curve and AUC --

The ROC Curve shows the rate of the model false positive by true positive 
predictions over a range of threshold values, from 0 to 1. Here, it is used to quantify 
the predicitve power of this model using the characteristic statistic, AUC (Area Under the Curve).


This ROC and AUC show a rather even ability for the model to differntiate 
the two predicted classes with 80% probability.

- Cost Benefit Matrix - 

Info:
Statistical Average for Mobile Gaming is: adCPM $5 (One adwatch generates $0.005)
Assumption: Spending coins generates 10 times more revenue compared to adwatch.
Cost benefit matrix baseline: Do nothing if predict positive(the player will spend coins)

tp = 0 (do nothing)
fn = P(addoption == 0) * ((adwatch_revenue * P(adoption==1))- Missed Revenue From Coins)
tn = P(addoption == 0) * P(adwatch | adoption) * adwatch_revenue
fp = 0 (do nothing)

tp = model predicted that the player will spend coin and player spent
tp reaction --> do not provide adOption, probability of a player to spend coin is higher

fn = model predicted that the player will not spend coin, but actually the player was going to
fn reaction --> provide ads(decreases chances of spending coin)

tn = model predicted that the player will not spend coin, and player did not
tn reaction --> provide adOption(so company still generates some money)

fp = the model predicted that the player is going tp use coins, yet the player did not.
fp reaction --> do not provide adOption(missed )

Profit Curve:

tresholds created: linspace from 0 to 100
profit maximizing treshold index number : 57 (which stands for 57% treshold)

confusion matrix results with 57% treshold(# of labelled predictions)
tp: 18857
fp: 120359  --> this is sad(positive predictions)

fn: 120359
tn: 148730 --> this is not very good either(negative predictions)

-- Conclusions -- 

We have found a more effective way compared to simply blending adOption to the offer by 21% random chance. 

Instead, logistic model findings states that increasing the adOffer ratio to 42% based on the 57.7% treshold and following conditions maximizes the profit.

if;
- playtime of the player is high in terms of seconds for the particular level that player died
- the player leveled up through game seasons
- the number of days since the player installed the game is significant
- the game mode is set to causal mode
- the percentage left towards the end of the level is low
- player failed for a couple of times in a particular level before offer
- the player already have enough coins to proceed in his bank(more than 4,5 or 6 coins)


***Data Science Related Reminders:***

Datasets with unbalanced binary targets are tricky because
the model learns more from the dominant label which usually leads to overfit. 
Consequently model predicts as if there is only the dominant option which means
the signal for the weaker option is set to 0.

We observed this within our dataset in which the ratio of players
which do not pay is significantly higher compared to players that accept the coin option. 

SMOTE is a way to train a more balanced dataset for the prediction models.
On the other hand it may not be effective for each and every dataset.
In our case SMOTE actually worked fine which generated signal for positive predictions.
However the question is, did it really do a good job? 

I don't think so due to the high number of false positive predictions.
Regardless of that, it increased the signal for positive predictions which gave us a business solution.



-----   Appendix  -------------------------------------------------------------------------------------------------------------------------------------

    Dataset 1: SaveMe
{
  "_id": {
    "$oid": "58c80b93c1a7ac04b0a08f3b"
  },
  "offerReason": "Percentage",
  "percentageLeft": 0,
  "twentileLeft": 0,
  "hasEnoughCoin": 1,
  "HCost": 4,
  "adOption": 0,
  "numFails": 2,
  "playTime": 19,
  "levelMode": "Casual",
  "levelNumber": 163,
  "result": "reject",
  "ts": {
    "$date": {
      "$numberLong": "1489505171825"
    }
  },
  "playerId": "58bac635569af004b035b6cb",
  "hcl": {
    "Casual": 162,
    "Alternative": 65
  },
  "daysSinceInstall": 10,
  "cohort": {
    "day": 424,
    "week": 60
  },
  "sessions": 25,
  "platform": "IOS",
  "segments": {
    "IAP": "false",
    "ISG_PBS_Price": "Low",
    "ISG_SaveMe_Max_Percentage": "Mid",
    "ISG_EnergyRefill": "Low",
    "Country": "SA",
    "LanguageCode": "en"
  }
}

    Dataset 2: Last active level
    Last active level:

    {
      "_id": "5a537e91b20e6404ffd3c779",
      "ts": {
        "$date": {
          "$numberLong": "1537882136440"
        }
      },
      "levelNumber": 430,
      "levelStatistics": {
        "playCount": 1,
        "completeCount": 1,
        "quitCount": 0,
        "totalDamage": 2,
        "damageBeforeFinishLevel": 2,
        "totalFail": 0,
        "failBeforeLevel": 0,
        "minCompleteTime": 53,
        "firstCompleteTime": 53,
        "totalPlayTime": 53,
        "totalPlayTimeBeforeFinishLevel": 53,
        "iaps": {},
        "softPurchasedVGoods": {},
        "hardPurchasedVGoods": {},
        "usedVGoods": {},
        "energyCount": 5,
        "lastDifficulty": 0,
        "location": "LevelComplete",
        "ts": {
          "$date": {
            "$numberLong": "1537882136436"
          }
        }
      },
      "playerData": {
        "segments": {
          "IAP": "false",
          "ISG_SaveMe_Enabled": "Disabled",
          "ISG_LEVEL_DIFFICULTY": "Normal",
          "Country": "KW",
          "LanguageCode": "en",
          "ISG_DIFF_ADJUSTMENT": "Legacy"
        },
        "goods": {
          "SaveMeSword": {
            "$numberLong": "14"
          },
          "PowerSlash": {
            "$numberLong": "3"
          },
          "Thunderbolt": {
            "$numberLong": "2"
          },
          "DragonBreath": {
            "$numberLong": "1"
          }
        },
        "hardCoin": {
          "$numberLong": "7"
        },
        "softCoin": {
          "$numberLong": "48500"
        },
        "money": {
          "$numberLong": "0"
        },
        "sessions": 371,
        "daysSinceInstall": 268,
        "cohort": {
          "day": 734,
          "week": 104
        },
        "created": 1515421329405,
        "currentPlatform": "IOS",
        "currentCountry": "KW",
        "buildNumber": 239
      },
      "completed": true,
      "lastSeen": {
        "$date": {
          "$numberLong": "1538595425478"
        }
      }
    }