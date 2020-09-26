# Predicting Car Accident Severity 


## 1. Introduction/Business Problem

This project will involve working on a case study where we will predict car accident severity, given that we have information about an accident that has just occurred. The stakeholders will be emergency operators, first responders, and healthcare workers. The reason why accident severity is vital to the stakeholders is that if there is an accident, then the first responders would react accordingly to the information about an accident. After inputting a description of an accident into a model, they will receive a probability of how likely an injury occurred in that accident. From that information, they can decide how many ambulances, firetrucks, or police to dispatch based on whether there was an injury or non-injury accident. The benefit of this information is to save lives and emergency resources.



## 2. Data

### 2.1 Data Description

Our dataset consists of collisions reported by the Seattle Police Department and recorded by traffic records from 2004 to the present time. Each row represents a collision, and the dataset is updated weekly according to the [metadata](https://github.com/analisahill/Coursera_Capstone/blob/master/Capstone_Metadata.pdf). 
Several columns in each row tell us about the accident severity. The severity code is a code that labels the severity of the collision. The severity code can be the following:

| Accident Type      | Severity code    
| :------------- | :----------: | 
| Property Damage| 1            |
| Injury         | 2            |



The severity code will be our labels in our supervised machine learning model. The following are the features that will be used in our model:

**Feature sets**
* Severity code 
* Collision type (*Angles*, *Sideswipe*, *Parked Car*, *Other*, *Cycles*, *Rear Ended*, *Head On*, *Unknown*, *Left Turn*, *Pedestrian*, and *Right Turn*. )
* Location
* Number of pedestrians 
* Number of cyclists 
* Number of vehicles
* Number of injuries
* Number of serious injuries
* Number of fatalities
* Date of incident
* Time of incident
* Road conditions (*Wet*, *Dry*, *Unknown*, *Snow/Slush*, *Ice*, *Other*, *Sand/Mud/Dirt*, *Standing Water*, *Oil*)
* Light conditions (*Daylight*, *Dark - Street Lights On*, *Dark - No Street Lights*, *Unknown*, *Dusk*, *Dawn*, *Dark - Street Lights Off*, *Other*, *Dark - Unknown Lighting*)
* Weather (*Overcast*, *Raining*, *Clear*, *Unknown*, *Other*, *Snowing*, *Fog/Smog/Smoke*, *Sleet/Hail/Freezing Rain*, *Blowing Sand/Dirt*, *Severe Crosswind*, *Partly Cloudy*)
* Other data that is described in
[metadata](https://github.com/analisahill/Coursera_Capstone/blob/master/Capstone_Metadata.pdf)


### 2.2 How Data Will Be Used To Solve the Problem

We will use the severity description (*Injury Collision* or *Property Damage Only Collision*) to determine the accident severity. For this study, *Property Damage Only Collision* is less severe than an *Injury Collision*. We want to build a model to determine which combination of conditions might lead to one of these two cases. The feature sets of this dataset are listed above. They will determine what combinations of these features are more likely to lead to an *Injury Collision* or *Property Damage Only Collision*.


### 2.3 Data Feature Selection

The final features/columns selected from the data frame are listed below.

**Features Kept**


* SEVERITYCODE (severity code)
* ROADCOND (road condition)
* LIGHTCOND (light condition)
* VEHCOUNT (vehicle count)
* WEATHER  (weather)
* ADDRTYPE (address type) 
* COLLISIONTYPE (collision type)
* JUNCTIONTYPE (junction type)


**Features Dropped**

* X
* Y
* OBJECTID
* INCKEY
* COLDETKEY
* REPORTNO
* STATUS
* INTKEY
* LOCATION
* EXCEPTRSNCODE
* EXCEPTRSNDESC
* SEVERITYDESC
* INCDATE
* INCDTTM
* JUNCTIONTYPE
* SDOT_COLCODE
* SDOT_COLDESC
* INATTENTIONIND
* UNDERINFL
* PEDROWNOTGRNT
* SDOTCOLNUM
* ST_COLCODE 
* ST_COLDESC
* SEGLANEKEY
* CROSSWALKKEY

The dropped data was either incomplete or involved written description of the accidents and scenarios. Perhaps some of these descriptions could be looked more closely at using NLP but shall be left for another project. The other data would have required more data mining and monetary resources to obtain useful information for those features.

### 2.4 Data Cleaning 

Some parts of the dataset needed to be cleaned. There were nan values in the following columns:


* LIGHTCOND (light condition)
* WEATHER (weather)
* ADDRTYPE (address type)
* JUNCTIONTYPE (junction type)
* COLLISIONTYPE (collision type)

Since these nan values and their rows only took up about 6% of the data, therefore, those rows were dropped. 


## 3. Methodolgy

### 3.1 Exploratory Data Analysis

#### Target Variable

Conveniently, our target was already labeled as a severity description code, but there was also a little more information about this data set. The total number of collision incidents in our dataset is 194,673. Here is a breakdown of the target:

|               | Counts| Percent of Collisions 
 :------------- | :----------: | :----------: |
|Property Damage Only Collision|136485|	70.1 
|Injury Collision|	58188|       29.9 	


<br /> 

![severitydescription_vs_counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/severitydescription_vs_counts.png?raw=true)

In the table and plot, we can see that a non-injury collision occurs ~70% of the time versus an injury collision ~30% of the time. Here, we can tell that we have an unbalanced dataset, but in the future, we want to make this more balanced by finding additional datasets to join with this data.

#### Break-down of collision types

The collision types for this dataset are labeled as: head-on, right turn, unknown, cycles, pedestrian, left turn, sideswipe, other, rear-ended, angles, and a parked car. This data shows that accidents involving parked cars occur 25.3% of the time, followed by accidents at an angle (18.3%) and then rear-ended (~18.0%). The lowest collision types were head-on (1.0%), right-turn (1.6%), and cycles (2.9%).


<br /> 


![Collision Type Histogram](https://github.com/analisahill/Coursera_Capstone/blob/master/images/collision_type_vs_counts.png?raw=true)

A key takeaway is that some of the less prevalent collisions may be associated with an injury. Since this dataset is 30% injury and 70% non-injury, it would make sense if the larger population of accidents (non-injury) occurred more with parked cars involved. 

#### Break-down of junction types and address types

Sometimes accidents, in general, can be related to the location or circumstances of where they occur. That is why it is essential to look at junction and address types to determine a specific road layout that leads to more accidents.

![addresstype vs counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/addresstype_vs_counts.png?raw=true)

The data shows that 66% of the accident occurs on a block, then 34% at an intersection, and less than 1% in an alley. Logically this makes sense because most driving experiences are not done in alleys. It would make more sense that they happen where there are many other cars present. Looking into the junction type, we broke down more about the block and intersection accidents shown below in the histogram.

From the data, it was calculated that around 48% of these accidents are mid-block and not related to intersections, which coincides with the address type data. The second prevalent is at an intersection or intersection related, confirmed by the address type histogram shown before this plot.


#### Break-down of road conditions, weather, and light conditions

Another contributing factor to an accident besides the road layout is other conditions such as weather, light, and other road conditions. We look further into these topics and see that there are also essential features in this data.

![Road conditions versus counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/roadcondition_vs_counts.png?raw=true)

Surprisingly, 66% of the accidents occurred when the road was dry and 25% when the road was wet. We delved into this further by looking at what the weather conditions.

![Weather versus counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/weather_vs_counts.png?raw=true)

The weather conditions also confirmed what is seen in the road conditions. It is reported that 59% of the accidents occurred during clear weather, and 17% occurred when there was rain. The weather is a little more informative than the road condition data because it also gives the overcast option, which contributes almost 15% of the accidents. To understand this more, we can look further into the light conditions for the accidents.


![light conditions versus counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/lightcondition_vs_counts.png?raw=true)

Around 61% of the accidents happened during the day, and almost 26% of the accidents happened at night with the street lamps on. So now, we can see that there is a relationship between the weather, light, and road conditions. Their top two percentage values are similar in ratio to each other, which could help separate whether someone was injured in an accident or not. 

#### Accident Vehicle counts 

The final but most essential histogram vital to this case-study is the vehicle count in the accidents. The more cars involved in an accident, the more likely someone is injured because more cars and people are involved and is reflected in the following histogram:

![vehicle counts](https://github.com/analisahill/Coursera_Capstone/blob/master/images/vehicle_counts.png?raw=true)

One thing to note is that as the number of vehicles involved in the accident increases, the number decreases and is most likely due to a lack of statistics and incidents where more than five cars involved are rarer. If we want to sample these numbers better, we need to obtain more data to see what happens when the vehicle count increases.


### 3.2 Models

The explored models were logistic regression, decision tree, and random forest for our initial investigations. Below is the classification report for all those models.


![classification report](https://github.com/analisahill/Coursera_Capstone/blob/master/images/classification_report.png?raw=true)


The ROC (receiver operator characteristic) curves and AUC (area under the curve) were evaluated for each model. The ROC curve calculates the true positive rate versus the false positive rate of our classifier. The ROC of each model looks relatively comparable; however, the AUC sheds a little more information. The closer the AUC is to 1, the better classifier it is. In the end, the random forest is a little better than the decision tree and logistic regression with an AUC coming in at 0.7703. 

![ROC](https://github.com/analisahill/Coursera_Capstone/blob/master/images/ROC.png?raw=true)

 We want to pick the random forest as our final model because, in this circumstance, if someone were injured, we would rather have the AUC as close as one as possible or have a higher true positive rate than a higher false-positive rate. Even if the random forest is the better model, the other models still give us information about what makes injury accidents more likely to happen. Therefore, in this study, we utilize all three models to understand the data and use the random forest as a final model.


## 4. Results and Discussion


### Logistic Regression

The logistic regression model gave some critical descriptions of the essential features that the model tells us about the difference between injury and no injury accidents. 

![lr feature importance](https://github.com/analisahill/Coursera_Capstone/blob/master/images/lr_feature_importance.png?raw=true)

In this bar plot, we can see that injury accidents are caused when the collision type involves a pedestrian or cyclist, according to the model. The next contributing factor is vehicle count. These are shown by the positive values on the bar plot. It makes logical sense that the more vehicles involved in an accident mean a higher probability of getting injured because more people are involved. It also makes sense that a non-injury incident happens most with parked cars. The negative value determines this non-injury accident on the bar plot. Next, we can go to the decision tree to see what information that gives us. 




### Decision Tree

The decision tree will tell us the order or hierarchy that will lead to an accident. We can plot the actual tree, which is shown below (click on the plot to be able to zoom in). 

![dtree](https://github.com/analisahill/Coursera_Capstone/blob/master/images/dtree_render.png?raw=true)

Below are some snapshots of the tree. The first two snapshots show the hierarchy of what needs to occur if there is an injury accident, and the third is what leads to a non-injury accident.

**Injury Tree Snapshot 1**

![dtree_injury 1](https://github.com/analisahill/Coursera_Capstone/blob/master/images/dtree_render_injury1.png?raw=true)

**Injury Tree Snapshot 2**

![dtree_injury 2](https://github.com/analisahill/Coursera_Capstone/blob/master/images/dtree_render_injury2.png?raw=true)

**No Injury Tree Snapshot**

![dtree_no injury](https://github.com/analisahill/Coursera_Capstone/blob/master/images/dtree_render_no_injury1.png?raw=true)

### Random Forest

The random forest is considered our best model and will be used as our final model. Making a bar plot of the feature importance tells us how the model used those features to decide between a non-injury accident and an injury accident. The random forest is similar to the decision tree and almost nearly the same, but weighted a little differently because of the model's ensemble nature, but gives us the same trend as with the decision tree.

![rf feature importance](https://github.com/analisahill/Coursera_Capstone/blob/master/images/rf_feature_importance.png?raw=true)

The bar plot result tells us that a parked car and pedestrian collision type plays the most considerable role in deciding whether it was an injury or no injury accident. These collision types confirm what we found in the logistic regression model. However, the logistic regression model tells us more about which features directly contribute to its negative and positive values in the bar plot.

#### Real-world Field Test of the Random Forest

To see how our random forest model will perform in the real world, we will choose some features and feed it into our model. For example, let us say that we are an emergency responder, and we get a call. We ask for the facts about the accident that has occurred (i.e., road condition, light condition, weather, address-type, collision type, junction type, vehicle count) and decide to dispatch ambulances, police, and other responders based on that information. For a test case, we can input the features:

* ROADCOND='Wet'
* LIGHTCOND='Dark - No Street Lights' 
* WEATHER='Fog/Smog/Smoke'
* ADDRTYPE='Intersection'
* COLLISIONTYPE='Pedestrian'
* JUNCTIONTYPE='At Intersection (but not related to intersection)'
* VEHCOUNT=2

After inputting these features into our model, we received a probability of injury of 73%. If an emergency operator had this number, they would dispatch an ambulance to the scene. Then if they knew the vehicle count, they would decide on how many ambulances to send. This scenario is a real-world example of how this model can be used in a typical scenario.

## 5. Conclusions

In this study, we looked at accident severity to predict if someone in an accident had the chance to be injured. The critical features that we identified injury accidents are caused when the collision type involves a pedestrian or cyclist. The next contributing factor is vehicle count. We built a logistic regression and decision tree models that were not as robust as the random forest. The random forest model can help first responders determine whether they should dispatch paramedics, police, or none. Future directions could help determine how many responders should be sent depending on the accident severity. 








