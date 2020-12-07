## Where we are

London, England, Captial of an old Empire that sailed forth to conquere the world in search of tea and spices only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer. Or does it? As a consequence of its history, London today is one of the most multicultural cities in Europe and consequenty also one of the most multiculinary ones. This allows us to analyse data about eating habits of modern Londoners to learn what on our plates divide us and what defines us. 

{% include
religion_clustering.html %}

In the following we want to analyse a data set of the Tesco Purchases of Tesco Loyality Card holders in districts of the Greater London metropolitain area, and combine this with information about the areas inhabitants collected by the Greater London census.  

Tesco is the largest overall supermarket chain and thus also the largest out of the four domestic chains in the UK. It has a wide spread all over the Greater London metropolitian area as can be seen below. 

{% include Store_Locations_London.html %}

Our goal is to learn about the interplay of food consumption and socio-economic circumstances and to be possibly predict socio-economic facts about an area based on knowledge of its consumption habits. 


To give a rough outline, we start of with a <a href='#PreCurs'>precursory look </a>into the data we have collected, and also look into the question: <a href='#WhoIsTesco'>"Who is a Tesco Customer"</a>. Following this, as a foundation for later analysis and to help with the understanding of the data we want to cluster areas based on socio-economic factors and look into their interplay. This also allows us to assign discrete inforamtion for later analysis. 
Following this, we aim to train predictive models that allows us to predict socio-economic facts about an area based on their consumption habits. 

## Why it is important

Why is food important, sounds like an easy question to answer, however besides the importance of having sufficent food, the modern day issue in developed countries is having a proper composition and balance of nutirents. An improper diet might not lead to inbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humors</a>, as ancient doctors believed, but has far more intertwined effect on the individuals health. 

This becomes also important as an issue of public health, as the UK as most other European Countries uses a socalised healthcare system, in which individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>. 

Common concerns in nutrion include high Saturated Fat consumption, high Sugar Consumption, low Fibre Consumption and a low distribution of nutrients, thus an unbalanced or skewed diet. 

## What the Data says

The original Tesco data set was created based on purchase histories of Tesco Loyality Card holders, that were then aggreagted based on Place of Residency of the card holder to different census areas in the Greater London area. The data set, while enormus with 411 million individual purchased items, does not allow to attribute the purchases to individuals or groups of individuals, i.e families. Thus instead the researchers opted for tracking consumption habits in the different areas based on an average area product, an abstraction of nutirent content for each product that is consumed in the area.   

The Cesus data we process is provided by the [London Data Stores](https://data.london.gov.uk/) and covers the Data from the most recent Census, 2011, which might seem far off, however is close to the collection year of the Tesco Data in 2015.  

<a id='PreCurs'></a>

## What we can learn from the Data

We start of with some precusory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights. Starting off, we applied spearman correlation to identify staticially significant, *(p < 0.05)*, rank correlations between socio-economic facts and nutrients. 

{% include Correlation_Nutirents_Different_Ethnicitites.html %}

Looking into the Data we can make already a few interesting observations. First off, we see that there is a strong correlation in multiple groups in regard to fibre, protein, alcohol and sugar consumption, and addionally divergence in regard to the nutrient diversity.


{% include Correlation_Nutirents_Income_Eductation_Ethnicitites.html %}


Addional insights can be won when looking into an areas alcohol consumption in relation to its inhabitans wealth and ethnic make up. A relationship that, not to spoil, but rather to cock Chekov's gun, can be observed in multiple models we used in analysis. 

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Interesting, especially in this context is that, if we divide the data up into the consumption of specific types of products consumed in an area, thus into Wine, Beer and Spirits. We can see the summit line previously observed only to re-occur in regard to Wine consumption, while the pattern is nearly inverted for consumption of Spirits and gets roughed up for the consumption of Beer. 

{% include 3D_Multi_Plot_Test.html %}


<a id='WhoIsTesco'></a>

### Who shops at Tesco

To answer this question we looked first into an important information that the researches provided with the dataset, the relative representativness that each area achieved. **Representativness** here represents the number of loyality card holding customers vs. the number of area residence. An area with a low representativness has few Tesco customers and thus prevents extrapolation, as the few Tesco Customers would then speak for the majority of Residence in an Area. *The similarity to the UKs first-past the post election system is left uncommented by the Author*. This however also allows us to look into the interrelationships that cause an area to have a high representativness and thus many Tesco Customers. Looking into the spearman correlation, statistically significant correlation between representativness and socio-economic factors can be observed in the following:     

{% include Correlation_Representativness_Norm_White.html %}

These results can bring an interesting explaination to results intially discovered by the orginal data set reseachers, which discovered a decreasing representativness and number of transactions in areas of Southern London. Especially areas going closer to the Home Counties. These areas are compromised of the old white middle class leaving the urban core of London in the wake of Suburbaniziation during the Post-War years, which was later followed by middle class migraint communities. 

{% include Map_Factory_Representativness.html %}


As an alternative hypothesis this can also be attributed, at least in part, to skewed data collection, as in the original data set a higher number of Tesco Stores from which the Data was collected were situated north of the Thames.

--- 

<a id='Londoners'></a>

## The *who* is *who* of London

As an important first step to explore the population of London, we need to subdivide the population of London into ares with high similarity in regard to socio-economic make-up. To achieve this, we conducted Agglomorative Hierachical Clustering, to identify most similar subgrouping of the population based on attributes, such as Education, Median Household Income, Ethnicity and Religion. We identified a number of groupings, i.e clusters, for the different attributes based on silhoutte score.   

{% include ethnicity_clustering.html %}


<a id='Labeling'></a>

### Giving the Child a Name

The groupings, however, only provide information about similarity between areas, not direct labels for each grouping. Thus we needed to conduct our own further analysis to identify good labels, representativ of the areas population. 

As the weather in London is rather Covid-ty around this time of the year and thus can't validate our findings in Person, we conduced our study of the inhabitans in the subgroubed areas, based both on information present in the data and verified our conclusion, with wider research on the areas. 

For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place. 

{% include education_clustering.html %}

One example case, was the groupings based on the education level of the areas population, which resulted in the model reaching a high silhoutte score for 3 clusters. For these three clusters we then analyzed the areas median number of inhabitants with a certain education level tracked in the census. For the first Grouping we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education. For the second Grouping we can see that these include the Areas with an exceptionally high student population, which we can then further confirm, as these areas are those directly located around the major univerisities in London. 

As an addional source for confirming our Clustering and to utilize in later analysis we reference an Grouping conducted for policy makers in the Greater London area, based on K-Means, which provides groupings based on a wider range of *(joint-)* attributes and deeper analysis of socio-economic trends in the UK.

To explore our clustering results on a cross-sectional basis, you can use the interactive map below:

{% include Multilayer_Population_Plot.html %}

<a id='Distribution'></a>

### Who am I, and if so, how many

Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.


<a id='Ensemble'></a>

## Can we go deeper?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic facts about areas based on food consumption habits. For this we used the data set on the MSOA Level, as it has more areas with higher representativness in comparsion to the LSOA level, while also providing a higher granularity, as the next higher granularity, Wards. As these group many diverse area together, smooting out intresting divergences.  


We started off with regression analysis to predict contious outcome values based on nutrional facts. To improve the performance, we removed outliers, the quality was suffiecent in regard to completeness and missing values, thus we focused on identifying and removing extreme values using [Local Outlier Fields](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf). 

The resulting regression model were able to predict contious variables such as median area income or the percentage of *BAME* inhabitants in an area with a high coeffiecent of determination of 0. Intrestingly, 
# Regression Results 

# RandomForest Introduction
To better address the difference in each area, we switched for the further models from a regression task to a classification task, based on the [clustering and labeling]() we conducted earlier. This allows us to capture and refine better the difference in each area from a single contious feature to the groupings.

For the now classification task, we choose to train a Random Forst Model, which incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapdeted feature selection to increase variety between trees. 

However, before we started the analysis an important observation we made, was the severe imbalance in class labels that can be observed after the grouping, this is an obvious consequence of the real world population falling into equal sized groups. 

{% include Donuts_Clusters.html %}

We address this issue by oversampling the minority labels, to improve the model performence, as Random Forst are in there base form preferentile towards the majority class. As oversampling we compared different techniques, such as random oversampling, which just entails drawing from the minorty lables samples with replacement. However this technique only multiplies minority labels to put more weight behind them, it does not create new syntheic data. For this, more advanced approaches, such as SMOTE and ADASYN can be used, which create new synthetic minorty label data samples based on interpolating between existing sampels. 

For the model training, we addionally implemented GridSearch with cross validation over a pre-selected parameter space. Important to note here is the focus on balanced accuracy as the main scoring function and using class weighting for the individual random forest trees. Both, we choose to improve cross-label performance and thus increase the fairness of the classifier making it less bias towards the majority class, as normal accuracy as a scoring function would. 


{% include Heatmap_Ethnicity_RandomForest_Products.html %}

# 
Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.

Based on the 


<a id='Responsible'></a>

### Can we go back?

A major concern with complex models such as random forests and boosting trees is that, while they perform really well and are robust, they present a level of obfuscation to the deriviation of their final result. For a human they present a near-to-*blackbox* that transforms an input into a output, without granting the individual understanding of its decision mechanism. 

Addressing this concern as Responsible Data Science has become an increasing issue, especially when working with data concering sensitive data both in regard to individuals and to societal analysis. 

To contribute our part, addionally to our previous analysis based on random forests, we tried to train models that provide a better explainablility to their result and thus get at least a cautious look into the black box.   

<a id='Trees'></a>

#### But I like Trees 

First of we started with, Decision Trees, which in there basic format allow for explainability in their decision process, however under the constraint that the decision tree does not increase to massivly in depth and general complexity. In our model training we again utilized balanced accuracy scoring, grid search, oversampling and class-weights, to boost model performance and inter-class fairness.  

{% include_relative images/treeviz.svg %} 

Looking into the Decision tree we can observe similar behavior to the 



<a id='AssociationRules'></a>

#### The OG Mining Buzzword

{% include AssociationRuleTable_Nutrients.html %}


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.

<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia: 

- The Royal Family of *Windsor* based on its german Ancestry, pre-WW1 named *von Saxen-Coburg und Gotha*, in comparsion to other UK families does celebrate Christmas on Christmas Eve, the 24th of December, not the morning of the 25th. 
- On related reason, the royal connection to the *House of Hannover* brought  over from Germany the tradition of christmas trees and introduced it to the British Ilse. 
- Until the Betting and Gaming Act of 1960, the only Sport allowed to be played on Christmas Day was Archery, following the Unlawful Games Act of 1541 to promote the usage of the Longbow in comparsion to less publicly useful sports, such as Football, ... pardon, Soccer. 
- Following the 31th of December the only person in the UK not needing a passport to legally travel into the EU is Queen Elizabeth II, as all UK passports are issued in her name, she does not requiere one.
- For a similar reason she does not requiere a drivers license, although she earned one for diving a lorry during the Blitz. 
- The word trivia is derived from the latin words *tri* and *via*, with the former meaning *three* and the later *road*. The romans, big into public infrastructure, build many roads and on places where multiple med were easy places to publish public and common knowledge, i.e trivial, knowledge. 

<a id='Conclusion'></a>

### Anything else? 

We wish you all a Merry Chistmas and a Happy New Year. May your Plum Pudding be scrumptious. 


