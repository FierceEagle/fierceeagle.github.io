
## Where we are

London, England, Capital of an old Empire that sailed forth to conquer the world in search of tea and spices, only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer. 
Or did it? 
As in, did it return to such a diet? 
As a consequence of its history, London is nowadays one of the most multicultural cities in Europe and consequently also one of the most multi-culinary ones. 
This allows us to analyse the consumption habits of modern Londoners to find out how our plates divide us and how they define us. 

In the following, we want to analyse a data set of purchases by *Tesco Loyalty Cardholders* in districts of the Greater London Metropolitan Area.
We then combine it with information on the areas' inhabitants, collected by the Greater London census. 
The aim is to obtain insights on the interplay between nutrition and product consumption and socio-economic realities of Londoners.  

### What is a Tesco?

Tesco is the largest supermarket chain in the UK and thus also the largest out of the four domestic chains *(Asda, Sainsbury's, Tesco and Morrisons)* in the UK. It is widely spread all over Greater London, as can be seen below. 

{% include Store_Locations_London.html %}

To give a rough outline, we start off with a <a href='#PreCurs'>precursory look</a> into the data we have collected.
We also look into the question <a href='#WhoIsTesco'>"Who is a Tesco Customer?"</a>. 
Following the initial overview, we develop our understanding of the data by <a href='#Londoners'>clustering</a> the areas we consider according to multiple socio-economic factors.
This also allows us to assign <a href='#Labeling'>discrete attributes</a> to areas which we use in later analyses. 
After obtaining labels for the areas, we train predictive models that allow us to <a href='#Ensemble'>predict</a> socio-economic facts about an area based on their consumption habits. 
In order to address the black box approach our chosen models to represent, we additionally choose to try to model the prediction using more explainable <a href='#Responsible'>models</a>. 
These include <a href='#Trees'>Decision Trees</a> and <a href='#AssociationRules'>Association Rules</a>.

## Why this is important

Why is food important, sounds like an easy question to answer. However, besides the importance of having sufficient food, the modern-day issue in developed countries is having a proper composition and balance of nutrients. An improper diet might not lead to imbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humours</a>, as ancient doctors believed, but has a far more intertwined effect on the individual's health.Common concerns in nutrition include high saturated fat consumption, high sugar consumption, low fibre consumption, and little variety in nutrients, i.e an unbalanced or skewed diet. 

This becomes also important as an issue of public health, as the UK like most other European Countries maintains a socialized healthcare system, in which individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>. Understanding the interplay of socio-economic and nutrition can allow better allocation and more group-specific efforts to curb negative trends in nutrition and thus to lower the severity and prevent negative health outcomes.  

## What the Data Says

The original Tesco data set was created based on purchase histories of Tesco Loyalty Card holders, which were then aggregated based on the place of residency of the card-holder to the different levels of granularity of census areas in the Greater London area. The data set, while enormous with 411 million individual purchased items, does not allow to attribute the purchases to individuals or groups of individuals, i.e families. On the one hand, because this would present a clear breach of privacy and on the other hand, because the cards don't carry this attributive information. Instead, the researchers opted for tracking consumption habits in the different areas based on an average area product, an abstraction of nutrient content for each product that is consumed in the area. This approach further balances out population differences in each of the collection areas as they are roughly equally sized and additionally filters out noise concerning special purchase habits, i.e tourist consuming convenience products in the centre city area, higher alcohol consumption near to a stadium, and so on, as purchases are only tracked for loyalty cardholders and attributed to their home area, not the purchase area. The three types of granularity we consider, Lower Super Output Areas **LSOAs**, Medium Super Output Areas **MSOAs**, and **Wards**, do not all address the issue of imbalanced population distribution. Especially Wards, which represent electoral boundaries and not census areas follow a flat peaked distribution.

{% include Population_Distribution.html %}

The Census data we process is provided by the [London Data Stores](https://data.london.gov.uk/) and covers data from the most recent census, 2011, which might seem far off, however, is close to the collection year of the Tesco data in 2015. Some shifts in population movement could have happened during those four years, however, we don't expect rapid changes in the socio-economic make-up of an area.  

{% include Multilayer_Nutrient_Plot.html %}

<a id='PreCurs'></a>

## What we can learn from the Data

We start with some precursory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights. Starting, we applied Spearman correlation to identify statistically significant, *(p < 0.05)*, rank correlations between socio-economic facts and nutrients. 

{% include Correlation_Nutirents_Different_Ethnicitites.html %}

Looking into the Data we can already make a few interesting observations. First off, we see that there is a strong correlation in multiple groups regarding fiber, protein, alcohol, and sugar consumption, and additionally divergence concerning the nutrient diversity. We can conjoin this information with further analysis regarding median household income, and the number of inhabitants with the highest degree being a secondary school degree (Level 2) or an academic degree (Level 4+). Especially the high distinctiveness between the latter two concerning carbohydrate and sugar consumption, nutrient diversity, and alcohol consumption. *Intelligentsia bibent, as my Latin teacher would say*. An additional interesting observation is a high correlation between fiber and alcohol and area wealth.  

{% include Correlation_Nutirents_Income_Eductation_Ethnicitites.html %}

Combining the former observations, in the next step, we can look into an area's alcohol consumption in a joint relation of its inhabitants' wealth and ethnic make up. We can observe a clear trend, which is most strongly in cases, where both attributes increase jointly. 

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Interesting, especially in this context is that, if we divide alcohol consumption up into the different types of alcohol-containing products, thus into wine and spirits. We can see the summit line previously observed only to re-occur concerning wine consumption, while the pattern is nearly inverted for the consumption of spirits.

{% include 3D_Multi_Plot_Spirits_Wine.html %}

<a id='WhoIsTesco'></a>

### Who shops at Tesco

To answer this question we looked first into a piece of important information that the researches provided with the dataset, the relative representativeness that each area achieved. **Representativeness** here represents the ratio of loyalty card-holding customers vs. the number of an area residence. An area with low representativeness has few Tesco customers and thus prevents extrapolation, as the few Tesco customers would then speak for the majority of residents in an area. *The similarity to the UKs first-past-the post election system is left uncommented by the Author*. The ratio was then normalized using a min-max-scaling to provide normalized representativeness, with the most representative area having representativeness of 1 and the least representative area having representativeness of 0. 

{% include Map_Factory_Representativness.html %}

Representativeness allows us to look into the interrelationships that cause an area to have high representativeness and thus many Tesco Customers, or rather loyalty card-holding customers. Looking into the Spearman correlation, a statistically significant correlation between representativeness and socio-economic factors can be observed for multiple groups, both positively and negatively:

{% include Correlation_Representativness_Norm_White.html %}

Most of the correlations are rather weak, however, they can bring an interesting explanation to results initially discovered by the original data set researchers, which discovered decreasing representativeness and number of transactions in areas of southern London. An interesting tidbit to add to this is that this seems to be the case, especially in areas closer to the Home Counties. These areas are compromised of the old white middle class leaving the urban core of London in the wake of suburbanization during the post-war years, which was later followed by middle-class immigrant communities. Additionally to consider in this regard is if the effect in this instance is based on the inhabitants of these areas choosing Tesco as their store, or if Tesco as a company chooses these inhabitants as their customer. Sadly without further information about Tescos modus operandi concerning store openings and there aimed for a customer base, we can not easily answer this question any further.

Furthermore to consider at least in part, is skewed data collection, as in the original data set a higher number of Tesco stores from which the data was collected were situated north of the Thames, which introduces a bias towards the more ethnically diverse areas of northern London and its urban core. 

### So we meet again Mr. Snow

Today is the day when the snake catches its tail. During the [1854 Cholera epidemic](https://en.wikipedia.org/wiki/1854_Broad_Street_cholera_outbreak) in London, the researcher John Snow applied techniques of data collection, [visualization](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Snow-cholera-map-1.jpg/1280px-Snow-cholera-map-1.jpg) and analysis to halt the spread of the virus, through this he became both one of the precursors of modern epidemiologists and the avant-garde of data science. Today, we feel honored that we can say that we are back at the beginning, analyzing water consumption in London during a pandemic. 

{% include Correlation_Water_Wealth.html %}

A question initially springing to our attention during analyzing the correlation between different socio-economic facts and product consumption, was why water consumption does strongly correlate with area median income and different ethnicities. Especially as in this case we are considering water purchased in supermarkets, not tap water. This result is somehow counter-intuitive: One would expect people with low income to drink less bottled water because it is more expensive. Early on this leads us to explore the interrelation between social facts and economic facts about an area.  

{% include Map_Factory_Water.html %}

A few working theories we developed during the analysis were:

Water purchased in supermarkets can be carbonated, tap water is not, the liking for the former could fall out different along cultural and culinary lines.

Missing a few crucial puzzle pieces in our investigation, one crucial flaw is that we only record consumption habits from supermarket purchases using loyalty cards, consequently, information about gastronomy consumption is missing and even small purchases might be missing. This can also represent itself in the consumption habits of Londoners.
The store locations from which the data was sourced are unevenly distributed over the Greater London area, one consequence could be that heavy products, such as water is more likely to be bought by people at these stores living close by, especially in a city where more people are relying on public transportation. As the areas were many stores located are the northern parts and the urban core, this could skew the picture towards ethnically diverse communities living in the area closer to the store.  
Water quality might differ in low income and high-income areas, based on housing age and maintenance quality, sadly water quality does not easily conform to census area boundaries based on the underlying infrastructure. *A [fact](https://www.ph.ucla.edu/epi/snow/watercompanies.html) known by John Snow*.
While digging deeper into this topic, we found that consumer’s drinking water habits are a yet unsolved mystery. A US [study](https://iwaponline.com/wp/article-abstract/19/1/1/20521/Mistrust-at-the-tap-Factors-contributing-to-public?redirectedFrom=fulltext) states that Hispanic, Black, and foreign-born inhabitants are more likely to state tap water was unsafe. Also, low-income households experienced more water insecurity and ethnic minorities were more likely to have negative previous experiences with tap water.
Another [study](https://onlinelibrary.wiley.com/doi/full/10.1111/coep.12088) finds that bottled water is often attributed to greater safety and better taste. 
However, these studies are from the US, so who cares!? London still has the [best water of them all](https://www.standard.co.uk/news/official-london-tap-water-is-the-best-in-britain-6835465.html) and there is no need to carry heavy packs of water around. *What would John Snow say to this?*

Sadly, while this is an interesting mystery, we are not able to find a definitive answer just now, however, we are encouraged to solve this rather enticing mystery. 



--- 

<a id='Londoners'></a>

## The *who* is *who* of London

As an important next step to explore the consumption habits of Londoners, we need to subdivide the population of London into areas with high similarity concerning socio-economic make-up. To achieve this, we conducted Agglomerative Hierarchical Clustering, to identify the most similar subgrouping of the population based on attributes, such as Education, Median Household Income, Ethnicity, and Religion. We identified several groupings, i.e clusters, for the different attributes based on high silhouette score, a metric comparing in-group vs. out-group similarity for each area.   

<a id='Labeling'></a>

### Giving the Child a Name

This approach, however, only provides information about the similarity between areas, not direct labels for each grouping. Thus we needed to conduct our further analysis to identify good labels, representative of the population of the area. 

As the weather in London is rather _Covidy_ around this time of the year and thus we can't validate our findings in person, we conducted our study of the inhabitants in the subgroup areas, based both on information present in the data and verified our conclusion, with wider research on the areas. For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place. 

{% include Map_Factory_Education.html %}

One example case was the groupings based on the education level of the population of the area, which resulted in the model reaching a high silhouette score for 3 clusters. For these three clusters, we then analyzed the area's median number of inhabitants with a certain education level tracked in the census. For the first grouping, we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education. For the second grouping, we can see that these include the areas with an exceptionally high student population, which we can then further confirm, as these areas are those directly located around the major university campuses in London. Analyzing our ethnicity clustering, we could compare it with work conducted for the Wikimedia Foundation, and were indeed able to discover similar population distribution structures as in their [population plots](https://en.wikipedia.org/wiki/Ethnic_groups_in_London). 

As a side-note, a clustering and labeling conducted for policymakers in the Greater London area is available, which provides groupings based on a wider range of *joint*-attributes and deeper analysis of socio-economic trends in the UK. However is sadly less applicable to us, as it only provides this information on an LSOA / OA level. However, it still provides some [interesting reading](https://data.london.gov.uk/dataset/london-area-classification) and further considerations for more refined clustering approaches on socio-economic data.

To address some pitfalls of our approach, the clustering is mainly conducted on distinct subsets of data, like ethnicity, religion, or household income, thus it neither addresses certain interplay, for example, the distinction between a rich or poor minority community, or the distinctions between Indian, Pakistani and Bangladeshi, as the data groups them under the joined label Asian, which furthermore also includes any South-East Asian and Central Asian. *The authors pride themselves in this approach outclassing traditional British methods for clustering socio-economic groups, the pen and ruler*. 

<a id='Distribution'></a>

{% include Sunburst_.html %}

An additional pitfall is that the approach leads to unbalanced label distribution, as certain distinct groups in society are less common and in some instances prevent extraction of subgroups altogether as they are far too dispersed over the whole set of areas and overshadowed by other groups. One example, missing areas with middle eastern, Sikhs, or Jewish majority. The effects of these pitfalls can be seen later on concerning the predictive models and will be addressed during the model learning stage. 

To explore our clustering results on a cross-sectional basis for each area, you can use the interactive map below:

{% include Multilayer_Population_Plot.html %}

---

<a id='Ensemble'></a>

## Can we go deeper?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic facts about areas based on food consumption habits. For this, we used the data set on the MSOA Level, as it has more areas with higher representativeness in comparison to the LSOA level, while also providing a higher granularity, as the next higher granularity, Wards. The latter would group multiple diverse areas, smoothing out interesting divergences.

We started with a regression analysis to predict continuous outcome values based on nutritional facts. To improve the model's performance, we removed outliers. While the general data quality was near perfect concerning completeness and missing values, we focused on identifying and removing extreme values regarding nutrient and product consumption using [Local Outlier Fields](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf). 

The resulting regression model was able to predict continuous variables such as median area income or the percentage of *BAME* (***B****lack*, ***A****sian* and ***M****inority* ***E****thnic*) inhabitants in an area with a high coefficient of determination of 0.6. Furthermore from the model, we could learn that most of the input variables have a significant impact on the prediction result and thus can derive that they have a predictive interplay with the target values.

### The *Sherwood* Forest

To better address the difference in each area, we switched for the further models from a regression task to a classification task, based on the clustering and labeling we conducted earlier. This allows us to capture and refine better the difference in each area than a single continuous feature, which cannot easily represent multiple disjoint ethnic groups, religions, or education structures.

For the now classification task, we choose to train a Random Forest Model, which incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapted feature selection to increase the variety between trees. Each tree in the model represents a layered application of simple rules, which when applied split each set of areas into two subgroups, that again can be split based on another feature in the next layer of the tree. However, before we started the analysis we need to address again the pitfalls of the clustering, which lead to obvious unequal labeling, i.e. many more areas have a Christian or white majority in comparison to a Hindu majority or an Asian majority. 

{% include Donuts_Clusters.html %}

We address this issue by oversampling the minority labels, to improve the model performance, as Random Forest is in their base form preferential towards the majority class. For oversampling, we compared different techniques, such as random oversampling, which just entails drawing from the minority labels samples with replacement. However this technique only multiplies minority labels to put more weight behind them, it does not create new synthetic data. For this, more advanced approaches, such as [SMOTE](https://www.jair.org/index.php/jair/article/view/10302) and [ADASYN](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4633969&casa_token=lmtht2WRwM4AAAAA:H5Mwc-PPkrfC6jvyhiWFGIX0zL5IVX5rpFgD55AHHGjbJlx5YSAK98Qc7aFttn-UZKGfm0xLBw&tag=1) have been developed, which create new synthetic minority label data samples based on interpolating between existing samples. This approach helped in multiple models to boost the performance, however, in some instance was not enough to allow for successful model training. To further handle the imbalance we choose to evaluate the model on how good it performs not simply looking at how often it was correct out of all its predictions but to look at its accuracy balanced for every subgroup. As an easy example, a model that would need to classify areas according to ethnic make up could in the base case be accurate by simply being good in predicting white areas and misclassify all other areas, as there are many white areas it would still predict mostly correct and have high accuracy, while it has a far worse balanced accuracy.

#### The Good ... 

Utilizing our methodology, we indeed were able to train some models that were rather effective in predicting at least some of the socio-economic facts about an area. Especially the model to predict majority religion in an area did perform well,achieving up to 93% balanced accuracy and over 95% general accuracy. To explain this high performance, we might attribute this to the high distinctiveness religion achieves in comparison to other groupings, as consumption habits between sub denominations, Sunni and Shia, might be still mainly defined by the main religion, i.e *halal* or following *Vedic* beliefs. This, for example, might not hold to a broader grouping done in the original census, i.e Asians or Blacks, where we might have more distinctive subgroups aggregated into a larger whole, i.e Indians and Pakistani, or, Jamaicans and Central-Africans. 

#### ... and the Bad

Sadly, not all of our models did perform as well, especially the model predicting ethnic make-up and to a lesser extent the model predicting area income.*This is, unfortunately, part of the reality of our approach to this kind of analysis, but we might still learn something from this*. Reasons for the ill performance could be multi-faceted, however, one obvious reason is our grouping and labeling methodology and the underlying data collection in the census. We already introduced the caveat that the terms used in the census data about ethnicity are especially broad, which might prevent the model from learning distinctions between the major groups as they are themselves made up of multiple smaller and distinct subgroups. Another facette might be that for wealth, simply addressing household median income cannot easily reflect more complex interplays regarding wealth, especially in a rather expensive city as London homeownership can play a leading role in the disparity between people with similar household income. Adding to this that policies, such as *[Right to Buy](https://en.wikipedia.org/wiki/Right_to_Buy)* can further tip the scale by granting long-time residents an economic advantage vs. later arrivals and short-time renters. Thus the grouping conducted might not capture enough information to form more distinctive groups regarding economics. Something to note regarding the model performance of both of these models, that they generally achieved still a high accuracy, as they were able to distinguish between the majority class and the minority classes, however, did perform rather bad regarding distinguishing the minority classes between each other as can be seen in the confusion matrix for the classifier predicting majority ethnicity. We can see that it is both rather *precise* (middle) and *recall*s the majority of the white majority areas (right), however, is bad especially identifying ethnically diverse areas, which get misclassified across the complete spectrum. 

{% include Heatmap_Ethnicity_RandomForest_Nutrients.html %}

*After addressing the training of the models, we now want to also find out some more about their inner working and what are the most impactful facts they used in their predictions.*

### Learning what the model says

Interpreting random forest models regarding their decision process is hard, as they present ensembles of 10's - 100's of decision trees learned in a way to increase variety in the decision process of the individual trees. *Talking about not to see the wood for the trees*. Consequently, the individual trees are already complex, and over the size of the ensemble, it is nearly incomprehensible. However, to at least get a feeling for the underlying process we can use the *feature importance*, the contribution of individual features, to the final model. Analyzing this for our two most accurate models, predicting religion based on nutrients and based on product consumption, we can make a few interesting observations. What we need to keep in mind when doing, however, is that based on the training of the individual trees, the occurrence of a feature is at least to an extend influenced by random chance, as only subsets of features are considered for each split, thus an important feature might just have bad luck in the lottery and gets chosen a lesser amount of time than other features. Consequently, feature importance needs to be considered with a grain of salt. 

{% include FeatureImportance_Religion_RandomForest_Nutrients.html %}

For the model based on nutrients, we can see that especially nutrient diversity, protein, and salt consumption are often chosen splitting features in the classifiers. Finding a rationale behind the importance however can appear rather hard, without knowledge of how they indeed split the groups up. We might find some superficial explanations, such as vegetarianism being more prevalent in Hindu circles and thus leading to a lower protein, i.e meat consumption. 

{% include FeatureImportance_Religion_RandomForest_Products.html %}

Looking into which specific kinds of products have high importance regarding splitting presents a few more insights. Especially, Fish, Dairy, Sweets, and Wine consumption are highly important in identifying the religious majority in an area. Once again we might create a hypothesis regarding a possible rationale behind their importances, i.e alcohol consumption is forbidden in conservative Islamic circles, milk products enjoy high liking in some Hindu circles, and so forth. However, while we can learn that these features are relevant in the final decision process, what we want to learn is how they do lead to the final decision we can observe? And especially, *In which way they make groups distinguishable from each other?*  

--- 

<a id='Responsible'></a>

## Can we go back?

To further improve on the performance of our classifier, we might now go with an even more complex model or refine our clustering approach, however in this instance we might not opt for this option, and instead ask ourselves, if we can go back. 

A major concern with complex models such as random forests and boosting trees is that, while they perform well and are robust, they present a level of obfuscation to the derivation of their final result. For a human, they present a near-to-*black-box* that transforms an input into an output, without granting the individual understanding of its decision mechanism. 
Addressing this concern as Responsible Data Science has become an increasing issue, especially when working with data concerning sensitive data both concerning individuals and to societal analysis. 
To contribute our part, additionally to our previous analysis based on random forests, we tried to train models that provide a better explainability to their result and thus get at least a cautious look into the black box.   

<a id='Trees'></a>

### But I still like Trees 

First of we started with Decision Trees, starting from the top root node the data is split based on different features into smaller groups that then again can be split further. Decision Trees through this allow for great explainability in their decision process, however under the constraint that the decision tree does not increase to massively in-depth and general complexity. In our model training, we again utilized balanced accuracy scoring, grid search, oversampling and class-weights, to boost model performance and inter-class fairness. Which on the other hand, however, also leads to getting far more complex and nested trees in comparison to a tree trained on accuracy. To address the issue of complexity and overfitting we utilized a pruning method, removing smaller and indecisive splits of the tree classifier. The resulting decision trees, were unsurprisingly still rather complex, however, provide better insights regarding their decision structure, with special interest laid on the most decisive cuts between labels. Many of the trees we trained, did perform worse than the random forest models for the same features and targets, however in many instances not to an extensive amount. For example, in a more extreme case, the decision tree pendant to our best performing random forest model achieves a balanced accuracy of 0.927 on the test set, while the random forest model achieves 0.93, thus only a marginal improvement. The random forest however achieves also a higher base accuracy and a better F1-score. *Let's look into the decision tree model*. 

{% include_relative images/Religion_Products.svg %}

First off, we can see that the first and most defined split is based on Sweets consumption, with many majority Christian areas having a lower fraction of consumption. For the next split, something interesting can be observed as splitting based on fish consumption separates the Muslim and Hindu majority communities near perfect. As already hypothesized earlier, we can observe that alcohol consumption indeed is a splitting feature, that is used to divide between Christian and majority Muslim communities, and also our hypothesis about higher dairy consumption finds at least some confirmation based on a split. Overall utilizing a decision tree model allows for a better exploration of the data, as now we can see the impact of a certain splitting feature from which we might now base further analysis and exploration. 

---

<a id='AssociationRules'></a>

### *Rule Britannia*

An alternative when looking for explainable models is the use of *association rules*. These derive rules in the form **antecedents ➞ consequents**, based on observations of common item sets. In this instance, we used the different labels of the groupings as items. Additionally, we added especially high and low consumption of certain products or proportions of a nutrient as further items. 

We used different measures used in association rule mining to evaluate the rules that we derived through the former item set design. However, given the way that item set and association rules are designed, we reach certain conflicts in using metrics, as they are also once again susceptible to the unbalanced distribution of labels. 

In our analysis, we focus on rules of the form **nutrients/products ➞ socio-economic descriptors**. This means that we sought out rules that let us derive the socio-economic implications of nutrient distribution.

Before looking at some of the rules we found, it is important to note a few things. Firstly, we found *a lot* of rules, more than 7,000 actually with confidence above 0.6, distributed across 21 consequent sets. This means that there were 21 groups, i.e. distinct subsets of socio-economic descriptors for which we found antecedents.

Instead of going through these rules by hand, which wouldn't have led to anything anyway, we tried to find sensible ways of reducing the number of rules we consider. Looking at the groups for which we had obtained rules, we noticed that some of them seemed to be very similar. Thus, we computed the set of all antecedents for every subgroup and then used *Jaccard similarity* to determine how close the set of antecedents were between groups. We then kept, for every subset of groups with pairwise Jaccard similarity greater than 0.8 the best representative, that is, the rules for the group with the highest lift.

For example, instead of having the groups {Working Class, Secondary School} and {Working Class}, we only kept the first one as their respective antecedent sets had a Jaccard similarity of 0.94.

To further reduce the selection of rules, we required every rule to have a confidence of at least 0.65 and a lift greater than 1.8.

This had some unintended consequences. We found after filtering that we had, in the process, lost all rules with support higher than 0.1. We then further observed there was a pretty strong dichotomy between rules. As a rule of thumb, a rule either had high support (>0.1) and on the other hand a low lift (< 1.2), or it had a high lift (> 2) and low support (<0.07). This comes down to the distinction between making general, rather weak statements about large populations and making very precise statements about small groups. In the end, we opted for the second approach, as rules with low lift simply don't make very strong statements.

From the remaining rules, we selected a few by hand, as putting them all here would probably triple the length of the page at the very least.

{% include AssociationRuleTable_Nutrients.html %}

These rules let us reinforce observations made beforehand. For example, we can see that the working class tends to have increased spirits consumption while generally avoiding wine, or that the working class has a high sweets consumption.

### *Land's End*

Today we explored the interrelation of socio-economics and food consumption in the Greater London area.  


#### To improve the Recipe 

After rounding up the analysis, we want to take a final look back and consider what can be done better and where we could extend further from the foundation we have built here.  

As a next step, What we might consider is looking deeper into the data to find conjoint clustering and groupings based on a wider range of socio-economic attributes. The most obvious example would be the differentiation between the major minorities in the UK, Indian, Pakistani, and Bangladeshi, which in our approach got clustered together based on the skewed singular ethnicity attribute. An example to improve this would be based on adding majority religion to the clustering as further attributes, as thus we could better differentiate between both groups, however, this might also introduce further side-effects, thus we kept it simpler for our initial analysis. Additionally, we could take geo-distance into account, following the assumption of communities with a similar make-up are centralized into one or few clusters in a city. *As long as Robert Moses is not coming along*. 

Further efforts could be spent looking into data on a deeper granularity. In our analysis, we focused on studying MSOA, as they provide high representativeness in the original Tesco data set, while also being more numerous than Wards. LSOA falls faster regarding representativeness, however could introduce new avenues to groupings, as smaller communities are not overshadowed by a larger area population. Thus, this could allow to extend the original groupings and to address more socio-economic categories, single vs. [*DINK*](https://en.wikipedia.org/wiki/DINK) vs. family households, population density and dwelling type, social welfare recipients, age structure, which were sadly less differentiable on the MSOA level. Concerning the models that we trained, more training data would especially help the Random Forest classifier, and while not solving the label imbalance, as this is to expect about human populations, but at least create more variety inside each category, which might be better than our approach of using synthetic data. 

Last, but not least, concerning the model training we conducted, further refinement to the methodology we used could be applied to improve the model's performance. For example, some consideration could be spent to utilize feature engineering, such as computing ratios between carbohydrates and sugars or fats and saturated fats. However, training the best model possible was not our main goal with the models we had in mind, as we wanted to use the models to help bootstrap us learning the reasons why they work given the data and which are the crucial factors in determining differences between groups based on nutrients and product consumption. 

<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia: 

The Royal Family of *Windsor*, because of its German ancestry and its pre-WW1 *von Sachsen-Coburg und Gotha* name, celebrate Christmas on Christmas Eve, the 24th of December, not on the morning of the 25th as proper Englishmen would do. On a related note, the royal connection to the *House of Hannover* brought over from Germany the tradition of Christmas trees and introduced it to the British Isles. 

Until the Betting and Gaming Act of 1960, the only sport allowed to be played on Christmas Day was archery. This was due to the Unlawful Games Act of 1541 which aimed to promote the use of the longbow rather than less publicly useful sports, such as football... Pardon, *soccer*. 

After the 31st of December, Queen Elizabeth II will be the only person in the UK not to need a passport to enter the EU legally, as all UK passports are issued in her name. This is the same reason why she does not require a driver's license to drive on British roads, even though she earned one for driving [a lorry during the Blitz](https://i.insider.com/5e8646eb1378e3116b2372a4?width=1100&format=jpeg&auto=webp).

*On Metatrivia*. The word trivia is derived from the Latin words *tri* and *via*, with the former meaning *three* and the later *road*. The Romans, big into public infrastructure, built many roads. Places, where three - or more - of these roads met, were convenient places to make public announcements and thus making the announcement common, i.e *trivial*, knowledge. 

<a id='Conclusion'></a>

### Anything else? 

Although it might date the creation of this data story a little bit, we wish you all Happy Holidays and a hopefully better 2021. Take an example from the



