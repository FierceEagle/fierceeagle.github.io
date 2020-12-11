{% include_relative images/charles-postiaux-Q6UehpkBSnQ-unsplash.jpg %}

## Where we are

London, England, Captial of an old Empire that sailed forth to conquere the world in search of tea and spices only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer. Or does it? As a consequence of its history, London today is one of the most multicultural cities in Europe and consequenty also one of the most multiculinary ones. This allows us to analyse data about eating habits of modern Londoners to learn what on our plates divide us and what defines us. 

In the following we want to analyse a data set of the Tesco purchases of Tesco Loyality Card holders in districts of the Greater London metropolitain area, and combine this with information about the areas inhabitants collected by the Greater London census to form a picture of the interplay between nutrition and socio-economic realities.  


### What is a Tesco?

Tesco is the largest supermarket chain in the UK and thus also the largest out of the four domestic chains *(Asda, Sainsbury's, Tesco and Morrisions)* in the UK. It has a wide spread all over the Greater London metropolitian area as can be seen below. 

{% include Store_Locations_London.html %}

To give a rough outline, we start of with a <a href='#PreCurs'>precursory look </a>into the data we have collected, and also look into the question: <a href='#WhoIsTesco'>"Who is a Tesco Customer"</a>. Following this, as a foundation for later analysis and to help with the understanding of the data we want to <a href='#Londoners'>cluster</a> areas based on socio-economic factors and look into their interplay. This also allows us to assign 
<a href='#Labeling'>discrete information</a> for later analysis. 
Following this, we aim to train predictive models that allows us to 
<a href='#Ensemble'>predict</a>  socio-economic facts about an area based on their consumption habits. To address the black box approach our choosen models represent, we addionally choose to try to model the prediction using more explainable models, such as Decision Trees and association rules. 

## Why it is important

Why is food important, sounds like an easy question to answer, however besides the importance of having sufficent food, the modern day issue in developed countries is having a proper composition and balance of nutirents. An improper diet might not lead to inbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humors</a>, as ancient doctors believed, but has far more intertwined effect on the individuals health. 

This becomes also important as an issue of public health, as the UK as most other European Countries maintains a socalised healthcare system, in which individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>. 

Common concerns in nutrion include high Saturated Fat consumption, high Sugar Consumption, low Fibre Consumption and a low distribution of nutrients, thus an unbalanced or skewed diet. 

## What the Data says

The original Tesco data set was created based on purchase histories of Tesco Loyality Card holders, that were then aggreagted based on the place of residency of the card holder to different level of granularity of census areas in the Greater London area. The data set, while enormus with 411 million individual purchased items, does not allow to attribute the purchases to individuals or groups of individuals, i.e families. On the one hand, because this would present a clear breach of privacy and on the other hand, because the cards don't carry this attributive information. Instead the researchers opted for tracking consumption habits in the different areas based on an average area product, an abstraction of nutirent content for each product that is consumed in the area. This approach further balances out population differnces in each of the collection areas as they are roughly equally sized and addioanlly filters out noise in regard to special purchase habits, i.e tourist consuming convenience products in the center city area, higher alcohol consumption near to a stadium and so on, as purchases are only tracked for loyality card holders and attributed to their home area, not the purchase area. The the three types of granularity we consider, Lower Super Output Areas **LSOAs**, Medium Super Output Areas **MSOAs**, and **Wards**, do not all address the issue of imbalanced population distribution. Especially Wards, which represent electoral boundries and not census areas follow a flat peaked distribution.

{% include Population_Distribution.html %}

The Cesus data we process is provided by the [London Data Stores](https://data.london.gov.uk/) and covers the Data from the most recent Census, 2011, which might seem far off, however is close to the collection year of the Tesco Data in 2015. Some trends of population movement can happen in the four year difference, however, we don't expect to rapid changes in the socio-economic make-up of an area.  

{% include Multilayer_Nutrient_Plot.html %}

<a id='PreCurs'></a>

## What we can learn from the Data

We start of with some precusory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights. Starting off, we applied spearman correlation to identify staticially significant, *(p < 0.05)*, rank correlations between socio-economic facts and nutrients. 

{% include Correlation_Nutirents_Different_Ethnicitites.html %}

Looking into the Data we can already make a few interesting observations. First off, we see that there is a strong correlation in multiple groups in regard to fibre, protein, alcohol and sugar consumption, and addionally divergence in regard to the nutrient diversity. We can conjoin this information with a further analysis in regard towards Median Household income, and the amount of inhabitants with the highest degree being a secondary school degree (Level 2) or an academic degree (Level 4+). Especially the high distinctivness between the latter two in regard to carbohydrate and sugar consumption, the nutrient diversity and alcohol consumption. *Intelligentsia bibent, as my latin teach would say*. An addional interesting observation is the high correlation between Fibre and Alcohol and area wealth.  

{% include Correlation_Nutirents_Income_Eductation_Ethnicitites.html %}

Combining the former observations, in the next step we will look into an areas alcohol consumption in relation to its inhabitans wealth and ethnic make up. A relationship that, not to spoil, but rather to cock Chekov's gun, can be observed in multiple models we used in analysis. 

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Interesting, especially in this context is that, if we divide the data up into the consumption of specific types of products consumed in an area, thus into Wine, Beer and Spirits. We can see the summit line previously observed only to re-occur in regard to Wine consumption, while the pattern is nearly inverted for consumption of Spirits. 

{% include 3D_Multi_Plot_Spirits_Wine.html %}

<a id='WhoIsTesco'></a>

### Who shops at Tesco

To answer this question we looked first into an important information that the researches provided with the dataset, the relative representativness that each area achieved. **Representativness** here represents the ratio of loyality card holding customers vs. the number of area residence. An area with a low representativness has few Tesco customers and thus prevents extrapolation, as the few Tesco Customers would then speak for the majority of Residence in an Area. *The similarity to the UKs first-past the post election system is left uncommented by the Author*. The ration was then normalized using min-max-scaling to provide a normalized representativness, with the most representativ area having a representativness of 1 and the least representativ area having a representativness of 0. 

{% include Map_Factory_Representativness.html %}

Representativnes allows us to look into the interrelationships that cause an area to have a high representativness and thus many Tesco Customers, or rather loyality card holding customers. Looking into the Spearman correlation, statistically significant correlation between representativness and socio-economic factors can be observed for mutliple groups, both postivly and negativly:

{% include Correlation_Representativness_Norm_White.html %}

Most of the correlations are rather weak, however, they can bring an interesting explaination to results intially discovered by the orginal data set reseachers, which discovered a decreasing representativness and number of transactions in areas of Southern London. An interesting tidbit to add to this, is that this seems especially in areas closer to the Home Counties. These areas are compromised of the old white middle class leaving the urban core of London in the wake of Suburbaniziation during the Post-War years, which was later followed by middle class migraint communities. 

As an alternative hypothesis this can also be attributed, at least in part, to skewed data collection, as in the original data set a higher number of Tesco Stores from which the Data was collected were situated north of the Thames, which introduces a bias towards the more ethnically diverse areas of northern London.

--- 

<a id='Londoners'></a>

## The *who* is *who* of London

As an important first step to explore the population of London, we need to subdivide the population of London into ares with high similarity in regard to socio-economic make-up. To achieve this, we conducted Agglomorative Hierachical Clustering, to identify most similar subgrouping of the population based on attributes, such as Education, Median Household Income, Ethnicity and Religion. We identified a number of groupings, i.e clusters, for the different attributes based on high silhoutte score.   


<a id='Labeling'></a>

### Giving the Child a Name

The groupings, however, only provide information about similarity between areas, not direct labels for each grouping. Thus we needed to conduct our own further analysis to identify good labels, representativ of the areas population. 

As the weather in London is rather Covid-ty around this time of the year and thus we can't validate our findings in person, we conduced our study of the inhabitans in the subgroubed areas, based both on information present in the data and verified our conclusion, with wider research on the areas. 

For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place. 

{% include Map_Factory_Education.html %}

One example case was the groupings based on the education level of the areas population, which resulted in the model reaching a high silhoutte score for 3 clusters. For these three clusters we then analyzed the areas median number of inhabitants with a certain education level tracked in the census. For the first grouping we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education. For the second grouping we can see that these include the areas with an exceptionally high student population, which we can then further confirm, as these areas are those directly located around the major univerisity campi in London. Analysing our ethnicity clustering, we could compare it with work conducted for the Wikimedia foundation, and were indeed able to discover similar population distribution structures as in the [population plots](https://en.wikipedia.org/wiki/Ethnic_groups_in_London). 

As a sidenode, a Clustering and Labeling conducted for policy makers in the Greater London area is available, which provides groupings based on a wider range of *joint*-attributes and deeper analysis of socio-economic trends in the UK. However is sadly less applicable by us, as it only provides this information on an LSOA / OA level. However it still provides some [interesting reading](https://data.london.gov.uk/dataset/london-area-classification) and further considerations for more refined clustering approaches on socio-economic data.

To address some pitfalls of our approach, the clustering is mainly conducted on distinct subsets of data, like ethnicity, religion, or household income, thus it neither addresses certain interplay, for example the distinction between a rich or poor minority community, or the distictions between Indian, Pakistani and Bagladeshi, as the data groups them under the joined label Asian, which furthermore also includes any South-East Asian and Central Asian. *Facultative one could see this as the even more correct approach given the context of the UK*. 

{% include Sunburst_.html %}

An addional pitfall is that the approach leads to unbalanced label distribution, as certain distinct groups in a society are less common and in some instances prevents extraction of subgroups altogether as they are far to dispersed over the whole set of areas and overshadowed by other groups. One example, missing area with middle eastern, sikhs or jewish majority. The effects of these pitfalls can be seen later on in regard to the predicitve models, and will be addressed during the model learning stage. 

To explore our clustering results on a cross-sectional basis for each area, you can use the interactive map below:

{% include Multilayer_Population_Plot.html %}

<a id='Distribution'></a>

### Who am I, and if so, how many

Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.

Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.


<a id='Ensemble'></a>

## Can we go deeper?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic facts about areas based on food consumption habits. For this we used the data set on the MSOA Level, as it has more areas with higher representativness in comparsion to the LSOA level, while also providing a higher granularity, as the next higher granularity, Wards. The latter would group multiple diverse areas together, smoothing out intresting divergences.

We started off with regression analysis to predict contious outcome values based on nutrional facts. To improve the performance, we removed outliers. While the general data quality was near perfect in regard to completeness and missing values, we focused on identifying and removing extreme values using [Local Outlier Fields](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf). 

The resulting regression model were able to predict contious variables such as median area income or the percentage of *BAME* (***B****lack*, ***A****sian* and ***M****inority* ***E****thnic*) inhabitants in an area with a high coeffiecent of determination of 0.6. Furthermore from the parameters we can learn that most of the input variables have a  significant impact on the prediction result and thus can derive that they have an predictive interplay with the target values.
 
### The *Sherwood* Forest

To better address the difference in each area, we switched for the further models from a regression task to a classification task, based on the [clustering and labeling]() we conducted earlier. This allows us to capture and refine better the difference in each area from a single contious feature to the groupings.

For the now classification task, we choose to train a Random Forst Model, which incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapdeted feature selection to increase variety between trees. 

However, before we started the analysis we need to address again the pitfalls of the clustering, which lead to obvious unequal labeling, i.e many more areas have a christian or white majority in comparsion to a asian majority or a hindu majority. 

{% include Donuts_Clusters.html %}

We address this issue by oversampling the minority labels, to improve the model performence, as Random Forst are in there base form preferentile towards the majority class. As oversampling we compared different techniques, such as random oversampling, which just entails drawing from the minorty lables samples with replacement. However this technique only multiplies minority labels to put more weight behind them, it does not create new syntheic data. For this, more advanced approaches, such as [SMOTE](https://www.jair.org/index.php/jair/article/view/10302) and [ADASYN](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4633969&casa_token=lmtht2WRwM4AAAAA:H5Mwc-PPkrfC6jvyhiWFGIX0zL5IVX5rpFgD55AHHGjbJlx5YSAK98Qc7aFttn-UZKGfm0xLBw&tag=1) have been developed, which create new synthetic minorty label data samples based on interpolating between existing sampels. This approach helped in multiple models to boost the performance, however, in some instance was not enough to allow for succesful model training. Espeically in regard to Education, as too few areas represent Student majority areas and the model thus isn't able to learn to distinguish them.  

For the model training, we addionally implemented GridSearch with cross validation over a pre-selected parameter space. As a scoring function, we utilize balanced accuracy, the average class-wise recall, as the main scoring function instead of normal accuracy and using class weighting for the individual random forest trees. Both, we choose to improve cross-label performance and thus increase the fairness of the classifier making it less bias towards the majority class, as normal accuracy as a scoring function would. 

The resulting models achieved good performance for the diffent labelings after applying the previous mentioned transformations and model adaptions. The model based on nutrion that we used to predict ethnicity achieved an balanced accuracy of around 0.615, and an accuracy of 0.700 overall on a 6-class multi-classification problem. The best performing model was the model on predicting Religion, that achieved after application of stratified sampling, an balanced accuracy score of 0.84 on a 3-Class classification task, an improvment achieved through oversampling and Gridsearch, as the base model only reached a balanced accuracy of around 0.46. For the other models we reached balanced accuracy between 0.55 and 0.6, with especially the model to predict Education suffering in performance from not being able to distinguish student areas. Exploring the confusion matricies, we furthermore can learn more closely about the models performance, by looking into its prediction results:  

{% include Heatmap_Ethnicity_RandomForest_Nutrients.html %}

What we can learn from this is that in case the model misclassified a test example, the misclassification was still close to the original label, for example many of the wrongly classified area with a *stong black minority* were classified as an area with a *black majority*. This, however, does not explain all error in the model and can be based on the input not capturing enough information to make a clear classification and also the previous grouping step not creating distinctive enough groupings.

### Learning what the model says

Interpreting random forest models in regard to their decision process is hard, as they present ensembles of 10's - 100's of decision trees learned in a way to increase varity in the decision process of the individual trees. *Talking about not to see the wood for the trees*. Consequently the individual trees are already complex, and over the size of the ensemble it is incomprehensible. However, to at least get a feeling for the underlying process we can use the *feature importance*, the contribution of individual features, to the final model. 

{% include FeatureImportance_Ethnicity_RandomForest_Products.html %}

From this example we were able to learn that for the model predicting ethnicity based on product consumption, the consumption of water, red meats and wine are often choosen splitting features in the individual trees of the classifier. Thus these features provide good point to differtiate groups, in this case the ethnic make-up, of an area. However we can not easily learn what the differentation founding on the feature is, i.e how we would split the population based on it. Another example, in this case for an classifier trained on nutrional facts and areas religion: 

{% include FeatureImportance_Religion_RandomForest_Nutrients.html %}


# Discussion RANDOMFOREST

<a id='Responsible'></a>

Here we can see, 

However again we have the same problem, while we can learn that these features are relevant in the final decision process, how do they lead to the final decision. *How do these features make groups distinguishable from each other?*  


### Can we go back?

To further improve on performance of our classifier, we might now go with an even more complex model or refine our clustering approach, however in this instance we might not opt for this option, and instead ask ourself, if we can go back. 

A major concern with complex models such as random forests and boosting trees is that, while they perform really well and are robust, they present a level of obfuscation to the deriviation of their final result. For a human they present a near-to-*blackbox* that transforms an input into a output, without granting the individual understanding of its decision mechanism. 
Addressing this concern as Responsible Data Science has become an increasing issue, especially when working with data concering sensitive data both in regard to individuals and to societal analysis. 
To contribute our part, addionally to our previous analysis based on random forests, we tried to train models that provide a better explainablility to their result and thus get at least a cautious look into the black box.   

<a id='Trees'></a>

#### But I still like Trees 

First of we started with Decision Trees, which in there basic format allow for great explainability in their decision process, however under the constraint that the decision tree does not increase to massivly in depth and general complexity. In our model training we again utilized balanced accuracy scoring, grid search, oversampling and class-weights, to boost model performance and inter-class fairness. Which on the other hand, however, also lead to getting far more complex and nested trees in comparsion to a tree trained on accuracy. To address the issue of complexity and overfitting we utilized a pruning method, removing smaller and indecisive splits of the tree classifier. The resulting decision trees, were unsurprisingly still rather complex, however provide some insight in regarde to their decision structure, with special interest layed on the most decisive cuts between labels. Many of the trees we trained, did perform worse than the random forest models for the same features and targets, however in many instances not to an extensive amount. For example in a more extreme case the following decision tree model achieves a balanced accuracy of 0.797 on the test set, while the random forest model did only 0.801, thus only a marginal difference. The random forest however achieves also a higher base accuracy and a better F1 score. *Lets look into the decion tree model*. 

{% include_relative images/Religion_Product.svg %}

First off, we can see that the fist and most definit split is based on the protein consumption, we can see that the majority of areas with a Christian majority have a higher protein consumption than the areas dominated by the other groups. In addition to this for the second layer splits, we can see that fat and nutrient consumption are the most decisive attributes. The former especially divides between the majority christian and majority muslim areas. And further down allows again for the divsion between Hindu and Muslim areas. 

{% include_relative images/Income_Product_dt.svg %}

<a id='AssociationRules'></a>

#### *Rule Britannia*

An alternative in regard to explainable models is the usage of Association Rules, that derive rules in the form $antecedents \rightarrow consequents$, based on observation of common item sets. In this instance, we used the different labels of the groupings as items and addionally, added especially high and low consumption of a certain product or proportion of a nutrient or nutrional fact as further items. 

We used different measures used in association rule mining to evaluate the rules that we derived through the former item set design. However, given the way that item set and association rules are designed, we reach certain conflicts in using metrics, as they are also once again susceptible to unbalanced distribution of item types. 

We focus in our analysis on rules in the form nutrient/product -> socio-economic fact, thus rules that let us derive a consequence, i.e a socio-economic fact, based on common area attributes.  


{% include AssociationRuleTable_Nutrients.html %}


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.

### *Land's End*

We explored the 


#### To improve the Recipie 

After rounding up the analysis, we want to take a final look back and consider what can be done better and were we could extend further from the foundation we have build here.  

As a next step, What we might consider is looking deeper into the data to find conjoint clustering and groupings based on a wider range of socio-economic attributes. The most obvious example would be the differentiation between the major minorities in the UK, Indian, Pakistani and Bangladeshi, which in our approach got clustered together based on the skewed singluar ethnicity attribute. An example to improve this, would be based on adding majority religion to the clustering as further attributes, as thus we could better differentiate between both groups, however this might also introduce further side-effect, thus we kept it simpler for our analysis.  

Further efforts could be spend looking into data on a deeper granularity. In our analysis we focused on studying MSOA, as they provide a high representativness in the origial Tesco data set, while also being more numerous than Wards. LSOA fall down faster in regard to representativness, however could introduce new avenues to groupings, as smaller communitites are not overshadowed by a larger area population. Thus, this could allow to extend the origial groupings and to address more socio-economic categories, single vs. [*DINK*](https://en.wikipedia.org/wiki/DINK) vs. family households, population density and dwelling type, social welfare receipients, age structure, which were sadly less differentiable on the MSOA level. In regard to the models that we trained, more training data would especially help the Random Forest classifier, and while not solving the label imbalance, as this is to expect in regard to human populations, but at least create more variety inside each category, which might be better than our approach of using synthetic data. 

Last, but not least, in regard to the model training we conducted, further refinement to the methodology we used could be applied to improve the models performance. For example some consideration could be spend to utilize feature enginieering, such as computing ratios between carbohydrates and sugars or fats and saturated fats. However, training the best model possible was not our main goal with the models we had in mind, as we wanted to use the models to help bootstrap us learning the reasons why they work given the data and which are the crucial factors in determining differences between groups based on nutrians and product consumption. 


#### 


<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia: 

The Royal Family of *Windsor* based on its german Ancestry, pre-WW1 named *von Saxen-Coburg und Gotha*, in comparsion to other UK families does celebrate Christmas on Christmas Eve, the 24th of December, not the morning of the 25th. 

On related reason, the royal connection to the *House of Hannover* brought  over from Germany the tradition of christmas trees and introduced it to the British Isle. 

Until the Betting and Gaming Act of 1960, the only Sport allowed to be played on Christmas Day was Archery, following the Unlawful Games Act of 1541 to promote the usage of the Longbow in comparsion to less publicly useful sports, such as Football, ... pardon, Soccer. 

Following the 31th of December the only person in the UK not needing a passport to legally travel into the EU is Queen Elizabeth II, as she is the offical issuer of all UK passports. 

For a similar reason she does not requiere a drivers license, although she earned one for driving a lorry during the Blitz.

The word trivia is derived from the latin words *tri* and *via*, with the former meaning *three* and the later *road*. The romans, big into public infrastructure, build many roads and places where multiple med were easy places to publish public and common knowledge, i.e trivial, knowledge. 

<a id='Conclusion'></a>

### Anything else? 

We wish you all a Merry Chistmas and a Happy New Year. May your Plum Pudding be scrumptious. 


