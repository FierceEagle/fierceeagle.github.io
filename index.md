
## Where we are

London, England, Capital of an old Empire that sailed forth to conquer the world in search of tea and spices only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer. Or does it? As a consequence of its history, London today is one of the most multicultural cities in Europe and consequently also one of the most multi-culinary ones. This gives us the opportunity to analyse eating habits of modern Londoners to find out how our plates divide us and how they define us. 

In the following we want to analyse a data set of purchases by Tesco Loyalty Card holders in districts of the Greater London metropolitan area, and combine it with information of the areas' inhabitants, collected by the Greater London census, to get insight into the interplay between nutrition and socio-economic realities.  


### What is a Tesco?

Tesco is the largest supermarket chain in the UK and thus also the largest out of the four domestic chains *(Asda, Sainsbury's, Tesco and Morrisons)* in the UK. It has a wide spread all over the Greater London metropolitan area as can be seen below. 

{% include Store_Locations_London.html %}

To give a rough outline, we start of with a <a href='#PreCurs'>precursory look </a>into the data we have collected, and also look into the question: <a href='#WhoIsTesco'>"Who is a Tesco Customer"</a>. Following this, as a foundation for later analysis and to help with the understanding of the data we want to <a href='#Londoners'>cluster</a> areas based on socio-economic factors and look into their interplay. This also allows us to assign 
<a href='#Labeling'>discrete information</a> for later analysis. 
Following this, we aim to train predictive models that allows us to 
<a href='#Ensemble'>predict</a>  socio-economic facts about an area based on their consumption habits. To address the black box approach our chosen models represent, we additionally choose to try to model the prediction using more explainable models, such as Decision Trees and association rules. 

## Why this is important

Why is food important, sounds like an easy question to answer. However, besides the importance of having sufficient food, the modern day issue in developed countries is having a proper composition and balance of nutrients. An improper diet might not lead to imbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humours</a>, as ancient doctors believed, but has far more intertwined effect on the individuals health. 

This becomes also important as an issue of public health, as the UK like most other European Countries maintains a socialised healthcare system, in which individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>. 

Common concerns in nutrition include high saturated fat consumption, high sugar consumption, low fibre consumption and a little distribution of nutrients, thus an unbalanced or skewed diet. 

## What the Data says

The original Tesco data set was created based on purchase histories of Tesco Loyalty Card holders, that were then aggregated based on the place of residency of the card holder to different level of granularity of census areas in the Greater London area. The data set, while enormous with 411 million individual purchased items, does not allow to attribute the purchases to individuals or groups of individuals, i.e families. On the one hand, because this would present a clear breach of privacy and on the other hand, because the cards don't carry this attributive information. Instead the researchers opted for tracking consumption habits in the different areas based on an average area product, an abstraction of nutrient content for each product that is consumed in the area. This approach further balances out population differences in each of the collection areas as they are roughly equally sized and addioanlly filters out noise in regard to special purchase habits, i.e tourist consuming convenience products in the centre city area, higher alcohol consumption near to a stadium and so on, as purchases are only tracked for loyality card holders and attributed to their home area, not the purchase area. The the three types of granularity we consider, Lower Super Output Areas **LSOAs**, Medium Super Output Areas **MSOAs**, and **Wards**, do not all address the issue of imbalanced population distribution. Especially Wards, which represent electoral boundaries and not census areas follow a flat peaked distribution.

{% include Population_Distribution.html %}

The Census data we process is provided by the [London Data Stores](https://data.london.gov.uk/) and covers data from the most recent census, 2011, which might seem far off, however is close to the collection year of the Tesco data in 2015. Some shifts in population movement could have happened during those four years, however, we don't expect rapid changes in the socio-economic make-up of an area.  

{% include Multilayer_Nutrient_Plot.html %}

<a id='PreCurs'></a>

## What we can learn from the Data

We start of with some precursory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights. Starting off, we applied spearman correlation to identify statistically significant, *(p < 0.05)*, rank correlations between socio-economic facts and nutrients. 

{% include Correlation_Nutirents_Different_Ethnicitites.html %}

Looking into the Data we can already make a few interesting observations. First off, we see that there is a strong correlation in multiple groups in regard to fibre, protein, alcohol and sugar consumption, and additionally divergence in regard to the nutrient diversity. We can conjoin this information with a further analysis in regard towards median household income, and the amount of inhabitants with the highest degree being a secondary school degree (Level 2) or an academic degree (Level 4+). Especially the high distinctiveness between the latter two in regard to carbohydrate and sugar consumption, the nutrient diversity and alcohol consumption. *Intelligentsia bibent, as my Latin teach would say*. An additional interesting observation is the high correlation between fibre and alcohol and area wealth.  

{% include Correlation_Nutirents_Income_Eductation_Ethnicitites.html %}

Combining the former observations, in the next step we can look into an areas alcohol consumption in a joint relation of its inhabitants wealth and ethnic make up. We can observe a clear trend, which is most strongly in cases, where both attributes increase jointly. 

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Interesting, especially in this context is that, if we divide alcohol consumption up into the different types of alcohol containing products, thus into wine and spirits. We can see the summit line previously observed only to re-occur in regard to wine consumption, while the pattern is nearly inverted for consumption of spirits.

{% include 3D_Multi_Plot_Spirits_Wine.html %}

<a id='WhoIsTesco'></a>

### Who shops at Tesco

To answer this question we looked first into an important information that the researches provided with the dataset, the relative representativeness that each area achieved. **Representativeness** here represents the ratio of loyalty card holding customers vs. the number of area residence. An area with a low representativeness has few Tesco customers and thus prevents extrapolation, as the few Tesco customers would then speak for the majority of residence in an area. *The similarity to the UKs first-past the post election system is left uncommented by the Author*. The ratio was then normalized using a min-max-scaling to provide a normalized representativeness, with the most representative area having a representativeness of 1 and the least representative area having a representativeness of 0. 

{% include Map_Factory_Representativness.html %}

Representativeness allows us to look into the interrelationships that cause an area to have a high representativeness and thus many Tesco Customers, or rather loyalty card holding customers. Looking into the Spearman correlation, statistically significant correlation between representativeness and socio-economic factors can be observed for multiple groups, both positively and negatively:

{% include Correlation_Representativness_Norm_White.html %}

Most of the correlations are rather weak, however, they can bring an interesting explaination to results intially discovered by the orginal data set reseachers, which discovered a decreasing representativness and number of transactions in areas of southern London. An interesting tidbit to add to this, is that this seems to be the case especially in areas closer to the Home Counties. These areas are compromised of the old white middle class leaving the urban core of London in the wake of suburbaniziation during the post-war years, which was later followed by middle class migraint communities. Addionally to consider in this regard is if the effect in this instance is based of the inhabitants of these areas choosing Tesco as their store, or if Tesco as a company choose these inhabitants as their customer. Sadly without further information about Tescos modus operandi in regard to store openings and their aimed for customer base, we can not easily answer this question any further.

Furthermore to consider at least in part, is skewed data collection, as in the original data set a higher number of Tesco stores from which the data was collected were situated north of the Thames, which introduces a bias towards the more ethnically diverse areas of northern London and its urban core. 

### So we meet again Mr. Snow

Today is the day when the snake catches its tail. During the [1854 Cholera epidemic](https://en.wikipedia.org/wiki/1854_Broad_Street_cholera_outbreak) in London, the researcher John Snow applied techniques of data collection, [visualization](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Snow-cholera-map-1.jpg/1280px-Snow-cholera-map-1.jpg) and analysis to halt the spread of the virus, through this he became both one of the precursors of modern epidemiologists and the avant-garde of data science. Today, we feel honoured that we can say that we are back at the beginning, analysing water consumption in London during a pandemic. 

{% include Correlation_Water_Wealth.html %}

A question initially springing to our attention during analysing correlation between different socio-economic facts and product consumption, was why water consumption does strongly correlate with area median income and different ethnicities. Especially as in this case we are considering water purchased in supermarkets, not tap water. 

A few working theories we developed during the analysis were:

Water purchased in supermarkets can be carbonated, tap water is not, the liking for the former could fall out different along cultural and culinary lines.

Missing a few crucial puzzle piece in our investigation, one crucial flaw is that we only record consumption habits from supermarket purchases using loyality cards, consequently information about gastronomy consumption is missing and even small purchases might be missing. This can also represent itself in consumption habits of Londoners.

The store locations from which the data was sourced are unevenly distributed over the Greater London area, one consequence could be that heavy produce, such as water is more likely to be bought by people at these stores living close by, especially in a city where more people are relying on public transportation. As the areas were many stores located are the northern parts and the urban core, this could skew the picture towards ethnically diverse communities living in the area closer to the store.  

Water quality might differ in low income and high income areas, based on housing age and maintainence quality, sadly water quality does not easily conform to census area boundaries based on the underlying infrastructure. *A [fact](https://www.ph.ucla.edu/epi/snow/watercompanies.html) known by John Snow*.

While digging deeper into this topic, we found that consumer’s drinking water habits are a yet unsolved mystery. A US [study](https://iwaponline.com/wp/article-abstract/19/1/1/20521/Mistrust-at-the-tap-Factors-contributing-to-public?redirectedFrom=fulltext) states that Hispanic, Black and foreign-born inhabitants are more likely to state tap water was unsafe. Also low-income households experienced more water insecurity and ethnic minorities were more likely to have negative previous experiences with tap water.
Another [study](https://onlinelibrary.wiley.com/doi/full/10.1111/coep.12088) finds that bottled water is often attributed to greater safety and better taste. 
However, these studies are from the US, so who cares!? London still has the [best water of them all](https://www.standard.co.uk/news/official-london-tap-water-is-the-best-in-britain-6835465.html) and there is no need to carry heavy packs of water around. *What would John Snow say to this?*

Sadly, while this is a interesting mystery, we are not able to find a definitive anwser just now, however we are encouraged to solve this rather enticing mystery. 

{% include Map_Factory_Water.html %}

--- 

<a id='Londoners'></a>

## The *who* is *who* of London

As an important next step to explore the consumption habits of Londoners, we need to subdivide the population of London into ares with high similarity in regard to socio-economic make-up. To achieve this, we conducted Agglomorative Hierachical Clustering, to identify most similar subgrouping of the population based on attributes, such as Education, Median Household Income, Ethnicity and Religion. We identified a number of groupings, i.e clusters, for the different attributes based on high silhoutte score, a metric comparing in-group vs. out-group similarity for each area.   

<a id='Labeling'></a>

### Giving the Child a Name

This approach, however, only provide information about similarity between areas, not direct labels for each grouping. Thus we needed to conduct our own further analysis to identify good labels, representative of the areas population. 

As the weather in London is rather _Covidy_ around this time of the year and thus we can't validate our findings in person, we conduced our study of the inhabitants in the subgroup areas, based both on information present in the data and verified our conclusion, with wider research on the areas. 

For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place. 

{% include Map_Factory_Education.html %}

One example case was the groupings based on the education level of the areas population, which resulted in the model reaching a high silhouette score for 3 clusters. For these three clusters we then analysed the areas median number of inhabitants with a certain education level tracked in the census. For the first grouping we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education. For the second grouping we can see that these include the areas with an exceptionally high student population, which we can then further confirm, as these areas are those directly located around the major university campuses in London. Analysing our ethnicity clustering, we could compare it with work conducted for the Wikimedia foundation, and were indeed able to discover similar population distribution structures as in their [population plots](https://en.wikipedia.org/wiki/Ethnic_groups_in_London). 

As a side-note, a clustering and labelling conducted for policy makers in the Greater London area is available, which provides groupings based on a wider range of *joint*-attributes and deeper analysis of socio-economic trends in the UK. However is sadly less applicable by us, as it only provides this information on an LSOA / OA level. However it still provides some [interesting reading](https://data.london.gov.uk/dataset/london-area-classification) and further considerations for more refined clustering approaches on socio-economic data.

To address some pitfalls of our approach, the clustering is mainly conducted on distinct subsets of data, like ethnicity, religion, or household income, thus it neither addresses certain interplay, for example the distinction between a rich or poor minority community, or the distinctions between Indian, Pakistani and Bangladeshi, as the data groups them under the joined label Asian, which furthermore also includes any South-East Asian and Central Asian. *The authors pride themselves in this approach outclassing traditonal british methods for clustering, the pen and ruler*. 

<a id='Distribution'></a>

{% include Sunburst_.html %}

An additional pitfall is that the approach leads to unbalanced label distribution, as certain distinct groups in a society are less common and in some instances prevents extraction of subgroups altogether as they are far to dispersed over the whole set of areas and overshadowed by other groups. One example, missing area with middle eastern, sikhs or jewish majority. The effects of these pitfalls can be seen later on in regard to the predictive models, and will be addressed during the model learning stage. 

To explore our clustering results on a cross-sectional basis for each area, you can use the interactive map below:

{% include Multilayer_Population_Plot.html %}


---

<a id='Ensemble'></a>

## Can we go deeper?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic facts about areas based on food consumption habits. For this we used the data set on the MSOA Level, as it has more areas with higher representativeness in comparison to the LSOA level, while also providing a higher granularity, as the next higher granularity, Wards. The latter would group multiple diverse areas together, smoothing out interesting divergences.

We started off with a regression analysis to predict continuous outcome values based on nutritional facts. To improve the models performance, we removed outliers. While the general data quality was near perfect in regard to completeness and missing values, we focused on identifying and removing extreme values in regard to nutrient and product consumption using [Local Outlier Fields](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf). 

The resulting regression model were able to predict continuous variables such as median area income or the percentage of *BAME* (***B****lack*, ***A****sian* and ***M****inority* ***E****thnic*) inhabitants in an area with a high coefficient of determination of 0.6. Furthermore from the model we could learn that most of the input variables have a significant impact on the prediction result and thus can derive that they have an predictive interplay with the target values.

### The *Sherwood* Forest

To better address the difference in each area, we switched for the further models from a regression task to a classification task, based on the [clustering and labelling]() we conducted earlier. This allows us to capture and refine better the difference in each area than a single continuous feature, which cannot easily represent mutliple disjoint ethnic groups, religions or education structures.

For the now classification task, we choose to train a Random Forest Model, which incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapted feature selection to increase variety between trees. Each tree in the model represents a layered application of simple rules, which when applied split each set of areas into two subgroups, that again can be split based on another feature in the next layer of the tree. 

However, before we started the analysis we need to address again the pitfalls of the clustering, which lead to obvious unequal labelling, i.e. many more areas have a Christian or white majority in comparison to a Hindu majority or a Asian majority. 

{% include Donuts_Clusters.html %}

<!-- We address this issue by oversampling the minority labels, to improve the model performance, as Random Forest are in there base form preferential towards the majority class. As oversampling we compared different techniques, such as random oversampling, which just entails drawing from the minority labels samples with replacement. However this technique only multiplies minority labels to put more weight behind them, it does not create new synthetic data. For this, more advanced approaches, such as [SMOTE](https://www.jair.org/index.php/jair/article/view/10302) and [ADASYN](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4633969&casa_token=lmtht2WRwM4AAAAA:H5Mwc-PPkrfC6jvyhiWFGIX0zL5IVX5rpFgD55AHHGjbJlx5YSAK98Qc7aFttn-UZKGfm0xLBw&tag=1) have been developed, which create new synthetic minority label data samples based on interpolating between existing samples. This approach helped in multiple models to boost the performance, however, in some instance was not enough to allow for successful model training. To further handle the imbalance we choose to evaluate the model on how good it performs not simply looking how often it was correct out of all its predictions, but to look at its accuracy balanced for every subgroup. As an easy example, a model that would need to classify areas according to ethnic make up could in the base case be really accuracte by simply being good in predicting white areas and simply misclassify all other areas, as there are many white areas it would still predict mostly correct and have a high accuracy, while it has a far worse balanced accuracy. -->

#### The Good ... 

Utilizing our methodology, we indeed were able to train some models that were rather effective in predicting at least some of the socio-economic facts about an area.  
Especially the model to predict majority religion in an area did perform well, achieving up to 93% balanced accuracy and over 95% general accuracy. To explain this especially high performance, we might attribute this especially to the high distinctiveness religion achieves in comparison to other groupings, as consumption habits between subdenominations, Sunni and Shia, of a branch religion might be still mainly defined by the main religion, i.e *halal* or *Aghnya*. This, for example, might not hold to a broader grouping done in the original census, i.e Asians, where we might have distinct subgroups, for example, Hindus and Muslim majority countries, aggregated into a larger whole, i.e Indians and Pakistani, or geographically distinct regions i.e Jamaicans and Central-Africans. 

#### ... and the Bad

Sadly, not all of our models did perform as well, especially the model predicting ethnic make-up and to a lesser extent the model predicting area income. Reasons for this might be multi-faceted, however, one obvious reason is our grouping and labeling methodology and the underlying data allocation. We already introduced the caveat that the census data about ethnicity is especially broad, which might prevent the model from learning distinctions between the major groups as they are themselves made up of multiple smaller and distinct subgroups. Another facette might be that for wealth, simply addressing household median income cannot easily reflect more complex interplays regarding wealth, especially in a rather expensive city as London homeownership can play a leading role in the disparity between people with similar household income. Adding to this that policies, such as *[Right to Buy](https://en.wikipedia.org/wiki/Right_to_Buy)* can further tip the scale by granting long-time residents an economic advantage vs. later arrivals and short-time renters. Thus the grouping conducted might not capture enough information to form more distinctive groups regarding economics. 

After training the model, we now want to also find out some more about its inner working and what are the most impactful facts it uses in its prediction.  

### Learning what the model says

Interpreting random forest models regarding their decision process is hard, as they present ensembles of 10's - 100's of decision trees learned in a way to increase variety in the decision process of the individual trees. *Talking about not to see the wood for the trees*. Consequently, the individual trees are already complex, and over the size of the ensemble, it is incomprehensible. However, to at least get a feeling for the underlying process we can use the *feature importance*, the contribution of individual features, to the final model. 

{% include FeatureImportance_Religion_RandomForest_Nutrients.html %}


{% include FeatureImportance_Religion_RandomForest_Products.html %}

However again we have the same problem, while we can learn that these features are relevant in the final decision process, how do they lead to the final decision. *How do these features make groups distinguishable from each other?*  






--- 

<a id='Responsible'></a>

## Can we go back?

To further improve on the performance of our classifier, we might now go with an even more complex model or refine our clustering approach, however in this instance we might not opt for this option, and instead ask ourself, if we can go back. 

A major concern with complex models such as random forests and boosting trees is that, while they perform really well and are robust, they present a level of obfuscation to the derivation of their final result. For a human they present a near-to-*black-box* that transforms an input into an output, without granting the individual understanding of its decision mechanism. 
Addressing this concern as Responsible Data Science has become an increasing issue, especially when working with data concerning sensitive data both in regard to individuals and to societal analysis. 
To contribute our part, additionally to our previous analysis based on random forests, we tried to train models that provide a better explainability to their result and thus get at least a cautious look into the black box.   

<a id='Trees'></a>

### But I still like Trees 

First of we started with Decision Trees, starting from the top root node the data is split based on different features into smaller groups that then again can be split further. Decision Trees through this allow for great explainability in their decision process, however under the constraint that the decision tree does not increase to massively in-depth and general complexity. In our model training we again utilized balanced accuracy scoring, grid search, oversampling and class-weights, to boost model performance and inter-class fairness. Which on the other hand, however, also lead to getting far more complex and nested trees in comparison to a tree trained on accuracy. To address the issue of complexity and overfitting we utilized a pruning method, removing smaller and indecisive splits of the tree classifier. The resulting decision trees, were unsurprisingly still rather complex, however provide some insight in regard to their decision structure, with special interest laid on the most decisive cuts between labels. Many of the trees we trained, did perform worse than the random forest models for the same features and targets, however in many instances not to an extensive amount. For example in a more extreme case the following decision tree model achieves a balanced accuracy of 0.797 on the test set, while the random forest model did only 0.801, thus only a marginal difference. The random forest however achieves also a higher base accuracy and a better F1-score. *Lets look into the decision tree model*. 

{% include_relative images/Religion_Product.svg %}

First off, we can see that the first and most defined split is based on protein consumption, we can see that the majority of areas with a Christian majority have a higher protein consumption than the areas dominated by the other groups. In addition to this for the second layer splits, we can see that fat and nutrient consumption are the most decisive attributes. The former especially divides between the majority Christian and majority Muslim areas. And further down allows again for the division between Hindu and Muslim areas. 

Another interesting tree, was the one based on product consumption used to predict area income. Analyzing the tree we can already discover some dividing lines between, 

{% include_relative images/Income_Product_dt.svg %}


---

<a id='AssociationRules'></a>

### *Rule Britannia*

An alternative in regard to explainable models is the usage of Association Rules, that derive rules in the form $antecedents \rightarrow consequents$, based on observation of common item sets. In this instance, we used the different labels of the groupings as items and additionally, added especially high and low consumption of a certain product or proportion of a nutrient or nutritional fact as further items. 

We used different measures used in association rule mining to evaluate the rules that we derived through the former item set design. However, given the way that item set and association rules are designed, we reach certain conflicts in using metrics, as they are also once again susceptible to unbalanced distribution of item types. 

We focus in our analysis on rules in the form nutrient/product -> socio-economic fact, thus rules that let us derive a consequence, i.e a socio-economic fact, based on common area attributes.  


{% include AssociationRuleTable_Nutrients.html %}


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.

### *Land's End*

Today we explored the interrelation of socio-economics and food consumption in the Greater London area.  

#### To improve the Recipie 

After rounding up the analysis, we want to take a final look back and consider what can be done better and were we could extend further from the foundation we have build here.  

As a next step, What we might consider is looking deeper into the data to find conjoint clustering and groupings based on a wider range of socio-economic attributes. The most obvious example would be the differentiation between the major minorities in the UK, Indian, Pakistani and Bangladeshi, which in our approach got clustered together based on the skewed singular ethnicity attribute. An example to improve this, would be based on adding majority religion to the clustering as further attributes, as thus we could better differentiate between both groups, however this might also introduce further side-effect, thus we kept it simpler for our analysis. Addionally, we could take geo-distnace into account, followign the assumption of communities with a similar make-up are centralized into one or few clusters in a city.

Further efforts could be spend looking into data on a deeper granularity. In our analysis we focused on studying MSOA, as they provide a high representativeness in the original Tesco data set, while also being more numerous than Wards. LSOA fall down faster in regard to representativeness, however could introduce new avenues to groupings, as smaller communities are not overshadowed by a larger area population. Thus, this could allow to extend the original groupings and to address more socio-economic categories, single vs. [*DINK*](https://en.wikipedia.org/wiki/DINK) vs. family households, population density and dwelling type, social welfare recipients, age structure, which were sadly less differentiable on the MSOA level. In regard to the models that we trained, more training data would especially help the Random Forest classifier, and while not solving the label imbalance, as this is to expect in regard to human populations, but at least create more variety inside each category, which might be better than our approach of using synthetic data. 

Last, but not least, in regard to the model training we conducted, further refinement to the methodology we used could be applied to improve the models performance. For example some consideration could be spend to utilize feature engineering, such as computing ratios between carbohydrates and sugars or fats and saturated fats. However, training the best model possible was not our main goal with the models we had in mind, as we wanted to use the models to help bootstrap us learning the reasons why they work given the data and which are the crucial factors in determining differences between groups based on nutrients and product consumption. 


#### 


<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia: 

The Royal Family of *Windsor* based on its German Ancestry, pre-WW1 named *von Sachsen-Coburg und Gotha*, in comparison to other UK families does celebrate Christmas on Christmas Eve, the 24th of December, not the morning of the 25th. 

On related reason, the royal connection to the *House of Hannover* brought  over from Germany the tradition of Christmas trees and introduced it to the British Isle. 

Until the Betting and Gaming Act of 1960, the only Sport allowed to be played on Christmas Day was Archery, following the Unlawful Games Act of 1541 to promote the usage of the Longbow in comparison to less publicly useful sports, such as Football... Pardon, Soccer. 

Following the 31st of December the only person in the UK not needing a passport to legally travel into the EU is Queen Elizabeth II, as all UK passports are issued in her name. 

For a similar reason she does not require a drivers license, although she earned one for driving a lorry during the Blitz.

The word trivia is derived from the Latin words *tri* and *via*, with the former meaning *three* and the later *road*. The Romans, big into public infrastructure, built many roads. Places where three (or more) of these roads met were convenient places to make public announcements and thus making them common, i.e _trivial_, knowledge. 

<a id='Conclusion'></a>

### Anything else? 

We wish you all a Merry Christmas and a Happy New Year. May your Plum Pudding be scrumptious. 


