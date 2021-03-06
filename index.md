
## Where We Are

*London, England*.
Capital of an old Empire that sailed forth to conquer the world in search of tea and spices, only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer.
Or did it?
As in, did it return to such a diet?
As a consequence of its history, London is nowadays one of the most multicultural cities in Europe and consequently also one of the most multi-culinary ones.
This allows us to analyse the consumption habits of modern Londoners to find out how our plates divide us and how they define us.

In the following, we want to analyse a data set of purchases by *Tesco Loyalty Cardholders* in districts of the Greater London Metropolitan Area.
We then combine it with information on the areas' inhabitants, collected by the Greater London census.
The aim is to obtain insights on the interplay between nutrition and product consumption and the socio-economic realities of Londoners.

### What is a Tesco?

Tesco is the largest supermarket chain in the UK and thus also the largest out of the four domestic chains *(Asda, Sainsbury's, Tesco and Morrisons)* in the UK. Today, it is widely spread all over Greater London Metropolitian Area, as can be seen below. Tesco today has multiple subsidaries and has entered the internation market. However today, we look into where it all began for Tesco, in London. *If you zoom in you might even find the original Tesco in **Hackney***.

{% include Store_Locations_London.html %}


### Where we are heading

In order to provide the reader with a rough outline, we begin the story with a <a href='#PreCurs'>precursory look</a> into the data we have collected.
This is accompanied by a look at the question <a href='#WhoIsTesco'>"Who is a Tesco Customer?"</a>.
Following the initial overview, we develop our understanding of the data by <a href='#Londoners'>clustering</a> the areas we consider according to multiple socio-economic factors.
This also allows us to assign <a href='#Labeling'>discrete attributes</a> to areas which we use in later analyses.
After obtaining labels for the areas, we train predictive models that allow us to <a href='#Ensemble'>predict</a> socio-economic facts about an area based on their consumption habits.
In order to address the black box approach our chosen models to represent, we additionally choose to try to model the prediction using more explainable <a href='#Responsible'>models</a>.
These include <a href='#Trees'>Decision Trees</a> and <a href='#AssociationRules'>Association Rules</a>.

## Why this is important

*"Why is food important?"* sounds like an easy question to answer.
However, besides the importance of having *enough* food, the modern-day issue in developed countries is having a proper composition and balance of nutrients.
An improper diet might not lead to imbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humours</a> as ancient doctors believed;
it has however far reaching effects on an individual's health.
Common concerns with regards to nutrition include high saturated fat consumption, high sugar consumption, low fibre consumption, and little variety in nutrients, that is, an unbalanced or skewed diet.

This also constitutes an issue of public health as the UK, like most other European countries, maintains a socialised healthcare system.
Therein, individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>.
Understanding the interplay of socio-economic factors and nutrition could allow for an improved resource allocation and more group-specific efforts to curb negative trends in nutrition;
this in turn would lower the severity and prevent negative health outcomes.
This would put off pressure from society and can also lead to higher individual happiness.

## What the Data Says

The original data set was created based on purchase histories of Tesco Loyalty Card holders, which were then aggregated based on the place of residence of the card-holder to the different levels of granularity of census areas in the Greater London area.
This data set, while extensive as it aggregates over 411 million individual purchased items, does not enable us to attribute the purchases to individuals or groups of individuals such as families.
This is due on one hand to the fact that it would constitute a **massive** breach of privacy; on the other hand, the cards do not actually provide this data in the first place.
Instead, the researchers opted for tracking consumption habits in the different areas based on an *average area product*, an abstraction of nutrient content for each product that is consumed in the area. Addionally they reported the relative fractions of different types of products consumed in each area.

This approach balances out population differences in each of the collection areas as they are roughly equally sized and additionally filters out noise concerning special purchase habits.
For example, tourist consumption is not taken into account as they generally do not have a loyalty card.
A higher alcohol consumption near stadiums would also be filtered out, as loyalty card purchases are attributed to the place of residence, not the place of purchase.

The three types of granularity we consider are Lower Super Output Areas (**LSOAs**), Medium Super Output Areas (**MSOAs**), and **Wards**.
These do not all address the issue of imbalanced population distribution.
Especially wards, which represent electoral boundaries rather than census areas follow a flat peaked distribution.

{% include Population_Distribution.html %}

The census data we process is provided by the [London Data Stores](https://data.london.gov.uk/) and covers data from the most recent census, 2011.
This might seem like antiquated data.
However, it is close to the collection year of the loyalty card data in 2015.
Some shifts in population movement may have happened during those four years, although we do not expect rapid changes in the socio-economic make-up of an area.

{% include Multilayer_Nutrient_Plot.html %}

<a id='PreCurs'></a>

## What We Can Learn From the Data

We start with some precursory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights and explore the data a little.

### Food for Thought
Starting with a correlation analysis, we applied Spearman correlation to identify statistically significant, *(p < 0.05)*, rank correlations between socio-economic facts and nutrients to identify possible pattern that we can observe and expand on further.

{% include Correlation_Nutirents_Different_Ethnicitites.html %}

We can already make a few interesting observations.
First, we see that there is a strong correlation in multiple groups regarding fibre, protein, alcohol, and sugar consumption.
Additionally we observe a divergence concerning the nutrient diversity.
We can conjoin this information with further analyses regarding median household income, and the number of inhabitants with the highest degree being a secondary school degree (Level 2) or an academic degree (Level 4+).
Especially the high distinctiveness between the latter two concerning carbohydrate and sugar consumption, nutrient diversity, and alcohol consumption.
*"Intelligentsia bibent", as my Latin teacher would say*.
Another interesting observation is the high correlation between fibre and alcohol consumption and area wealth.

{% include Correlation_Nutirents_Income_Eductation_Ethnicitites.html %}

Combining the former observations, we can look into an area's alcohol consumption in a joint relation of its inhabitants' wealth and ethnic make-up.
We observe a clear trend, most strongly in cases where both attributes increase jointly.

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Something interesting in this context is that we can divide alcohol consumption into the different categories of alcoholic products, that is, into wine and spirits.
We can see that the "summit line" previously observed for general alcohol consumption appears anew when observing wine consumption, while the pattern is nearly inverted with respect to spirit consumption.

{% include 3D_Multi_Plot_Spirits_Wine.html %}

<a id='WhoIsTesco'></a>

### Who Shops At Tesco

To answer this question we looked at an important piece of information that the researchers provided with the dataset: the relative representativeness that each area achieved.
Here, the *representativeness* of an area is defined as the ratio between the number of loyalty card-holding customers residing in it and its total number of inhabitants.
An area with low representativeness has few Tesco customers and thus prevents extrapolation, as the few Tesco customers would then speak for the majority of residents in an area.
*The similarity to the UKs first-past-the post election system is left uncommented by the Authors*.
The ratio was then normalised using a min-max-scaling to provide normalised representativeness, with the most representative area having a representativeness of 1 and the least representative area having a representativeness of 0.

{% include Map_Factory_Representativness.html %}

Representativeness gives us a measure to analyse why an area has a high or low number of Tesco customers, or rather, loyalty card-holding inhabitants.
Looking at the Spearman correlation, a statistically significant correlation between representativeness and socio-economic factors can be observed for multiple groups, both positively and negatively:

{% include Correlation_Representativness_Norm_White.html %}

Most of the correlations are rather weak.
Nevertheless, they can provide interesting explanations for the results initially obtained by the authors of the original paper.
There, they discovered a decreasing representativeness and number of transactions in areas of south of London.
An interesting tidbit to add to this is that this seems to be the case especially in areas closer to the Home Counties.
These areas are made up mostly of the old white middle class leaving the urban core of London in the wake of suburbanisation during the post-war years, which was later followed by middle-class immigrant communities.
A further hypothesis to be consider in this regard is the direction of this effect.
Put more plainly, do the inhabitants of these areas choose Tesco as their store, or does Tesco choose these inhabitants as their customers.
Sadly, without further information on Tescos modus operandi concerning store openings and their target customer base, we cannot easily answer this question.

Lastly, a fact that might at least bias the result is skewed data collection.
In the original data set, a higher number of Tesco stores from which the data was collected was situated north of the Thames.
This introduces a bias towards the more ethnically diverse areas of northern London and its urban core. However here the question might still stand, if this is a cause or an effect based on Tesco Modi operandi, or might even have another causes.

### So We Meet Again Mr. Snow

Today is the day the snake catches its tail. During the [1854 Cholera epidemic](https://en.wikipedia.org/wiki/1854_Broad_Street_cholera_outbreak) in London, the researcher John Snow applied techniques of data collection, [visualisation](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Snow-cholera-map-1.jpg/1280px-Snow-cholera-map-1.jpg) and analysis to halt the spread of the virus.
Thanks to his research, he became both one of the precursors of modern epidemiologists and the avant-garde of data science.
Today, we feel honoured in that we can say that we are back at the beginning, *analysing water consumption in London during a pandemic*.

{% include Correlation_Water_Wealth.html %}

A question sprang to our attention during our analysis of the correlation between different socio-economic factors and product consumption.
Why does water consumption strongly correlate with area median income and different ethnicities?
Especially as in this case we are considering water purchased in supermarkets, not tap water.
This result is somehow counter-intuitive: One would expect people with low income to drink less bottled water because it is more expensive.
Early on, this led us to explore the interrelations of social factors and economic factors of an area.

{% include Map_Factory_Water.html %}

A few working theories we developed during the analysis were:

Water purchased in supermarkets can be carbonated, tap water is not, a liking for the former could fall along different cultural and culinary lines.

For our investigation, we are missing a few crucial pieces of the puzzle.
One crucial flaw is that we only have records on consumption habits from supermarket purchases using loyalty cards.
Consequently, information about gastronomy consumption is missing and even small purchases might be missing.
This is also reflected in the consumption habits of Londoners.
The store locations from which the data was sourced are unevenly distributed over the Greater London area.
This could imply that heavy products such as water are more likely to be purchased by people living near a store.
This is especially possible in a dense city where many people rely on public transportation.
As the areas where many stores located are the northern parts and the urban core, this could skew the data towards ethnically diverse communities living in the area closer to a store.

Another idea is that water quality might differ between low and high-income areas, based on housing age and maintenance quality.
Sadly enough, water infrastructure, and by extension water quality, does not easily conform to census area boundaries.
*A [fact](https://www.ph.ucla.edu/epi/snow/watercompanies.html) known by John Snow*.
While digging deeper into this topic, we found that consumer's drinking water habits are a yet unsolved mystery.
A US [study](https://iwaponline.com/wp/article-abstract/19/1/1/20521/Mistrust-at-the-tap-Factors-contributing-to-public?redirectedFrom=fulltext) states that Hispanic, Black, and foreign-born inhabitants are more likely to think that tap water was unsafe.
Also, low-income households experienced more water insecurity and ethnic minorities were more likely to have negative previous experiences with tap water.
Another [study](https://onlinelibrary.wiley.com/doi/full/10.1111/coep.12088) finds that bottled water is often attributed to greater safety and better taste.
We must keep in mind that these studies are from the US, so who cares!?
London still has the [best water of them all](https://www.standard.co.uk/news/official-london-tap-water-is-the-best-in-britain-6835465.html) and there is no need to carry heavy packs of water around.
*What would John Snow say to this?*

While this is an interesting mystery, we are not able to find a definitive answer just now.
Nevertheless, we are encouraged to solve this rather enticing mystery.
*Stay hydrated*.

---

<a id='Londoners'></a>

## The *Who* is *Who* of London

As an important next step to explore the consumption habits of Londoners, we need to subdivide the population of London into areas with highly similar socio-economic make-ups.
To achieve this, we conducted *Agglomerative Hierarchical Clustering*.
This allows us to identify the most similar sub-groupings of the population based on attributes, such as Education, Median Household Income, Ethnicity, and Religion.
We identified several groupings, i.e clusters, for the different attributes based on high *silhouette score*, a metric comparing in-group and out-group similarity for each area.

<a id='Labeling'></a>

### Giving the Child a Name

This approach only provides information about the similarity between areas, not direct labels for each grouping.
Thus we needed to conduct further analyses to identify good labels, representative of the population of the area.

As the weather in London is rather _Covidy_ around this time of the year and thus we can't validate our findings in person, we conducted our study of the inhabitants in the subgroup areas, based on information present in the data.
We then verified our conclusion through wider research on the areas.
For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place.

{% include Map_Factory_Education.html %}

One example case was the groupings based on the education level of the population of the area, which resulted in the model reaching a high silhouette score for 3 clusters.
For these three clusters, we then analysed the area's median number of inhabitants with a certain education level tracked in the census.
For the first grouping, we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education.
For the second grouping, we can see that these include the areas with an exceptionally high student population.
This is further supported by the fact that these areas are those located directly around the major university campuses in London.
Analysing our ethnicity clustering, we could compare it with work conducted for the Wikimedia Foundation, and were indeed able to discover similar population distribution structures as in their [population plots](https://en.wikipedia.org/wiki/Ethnic_groups_in_London).

As a side-note, a clustering and labelling conducted for policy-makers in the Greater London area is available, which provides groupings based on a wider range of *joint*-attributes and deeper analysis of socio-economic trends in the UK.
For us it is sadly of little interest, as it only provides this information on an LSOA / OA level. However, it still provides some [interesting reading](https://data.london.gov.uk/dataset/london-area-classification) and further considerations for more refined clustering approaches on socio-economic data.

To address some pitfalls of our approach, the clustering is mainly conducted on distinct subsets of data, like ethnicity, religion, or household income, thus it neither addresses certain interplay.
For example, it fails to distinguish between a rich or poor minority community.
Another flaw is that Indian, Pakistani and Bangladeshi minorities, are grouped under a single joint *Asian* label, which additionally includes any South-East Asian and Central Asian minorities.
*The authors pride themselves in this approach, easily outclassing the traditional, tried and tested British method for clustering socio-economic groups, the pen and ruler*.

<a id='Distribution'></a>

{% include Sunburst_.html %}

An additional pitfall is that the approach leads to unbalanced label distribution, as certain distinct groups in society are less common and in some instances prevent extraction of subgroups altogether as they are far too dispersed over the whole set of areas and overshadowed by other groups.
For example, we are entirely missing areas with Middle Eastern, Sikh, or Jewish majorities.
The effects of these pitfalls can be seen later in the predictive models and will be addressed during the model learning stage.
To explore our clustering results on a cross-sectional basis for each area, you can use the interactive map below:

{% include Multilayer_Population_Plot.html %}

---

<a id='Ensemble'></a>

## Can We Go Deeper?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic properties of areas based on food consumption habits.
For this, we used the data set on the MSOA Level, as it has more areas with higher representativeness in comparison to the LSOA level, while also providing a higher granularity than the next higher granularity, Wards.
The latter would group multiple diverse areas, smoothing out interesting divergences.

We started with a regression analysis to predict continuous outcome values based on nutritional facts.
To improve the model's performance, we removed outliers.
While the general data quality was near perfect concerning completeness and missing values, we focused on identifying and removing extreme values regarding nutrient and product consumption using [*Local Outlier Fields*](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf).

The resulting regression model was able to predict continuous variables such as median area income or the percentage of *BAME* (***B****lack*, ***A****sian* and ***M****inority* ***E****thnic*) inhabitants in an area with a high coefficient of determination (0.6).
We could also learn that most of the input variables have a significant impact on the prediction result and thus can derive that they have a predictive interplay with the target values.

---

### The *Sherwood* Forest

To better address the differences between the areas in the following, we opted to switch from a regression task to a classification task based on the clustering and labelling we conducted earlier.
This allows us to better capture the differences between the areas than a single continuous feature, which cannot easily represent multiple disjoint ethnic groups, religions, or education levels.

For the next classification task, we chose to train a *Random Forest Model*.
It incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapted feature selection to increase the variety between trees.
Each tree in the model represents a layered application of simple rules, which when applied split each set of areas into two subgroups.
These can then again be split based on another feature in the next layer of the tree.
Before we start the analysis, we need to address the pitfalls of the clustering which led to obvious unequal labelling.
For example, many more areas have a Christian or white majority in comparison to a Hindu majority or an Asian majority.

{% include Donuts_Clusters.html %}

We address this issue by oversampling the minority labels to improve the model performance, as Random Forests are preferential towards the majority class.
We compared different oversampling techniques, such as random oversampling, which just entails drawing from the minority labels samples with replacement.
The disadvantage with this technique is that it only multiplies minority labels to put more weight behind them, it does not create new synthetic data.
More advanced approaches, such as [SMOTE](https://www.jair.org/index.php/jair/article/view/10302) and [ADASYN](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4633969&casa_token=lmtht2WRwM4AAAAA:H5Mwc-PPkrfC6jvyhiWFGIX0zL5IVX5rpFgD55AHHGjbJlx5YSAK98Qc7aFttn-UZKGfm0xLBw&tag=1) have been developed, which create new synthetic minority label data samples based on interpolating between existing samples.
This approach helped to boost the performance for multiple models, although in some instance it was not enough to allow for successful model training.

To further handle the imbalance we choose to evaluate the model on how good it performs not by simply looking at how accurate it was overall, but by looking at its *balanced accuracy* for every subgroup.
As an easy example, a model that would need to classify areas according to ethnic composition could in the base case be accurate by simply being good in predicting white areas and could misclassifying all other minority areas.
As there are many white areas it would still predict mostly correct and have high accuracy, while having a far worse balanced accuracy.

#### The Good...

With our methodology, we were indeed able to train some models that were effective in predicting at least some of the socio-economic properties of an area.
Especially the model to predict majority religion in an area did perform well, achieving up to 93% balanced accuracy and over 95% general accuracy.
This high performance could be attributed to the high distinctiveness religion achieves in comparison to other groupings.
In other words, consumption habits between sub-denominations, such as Sunni and Shia Muslims, might still be mainly defined by the main religion, i.e *halal* or following *Vedic* beliefs.
On the other hand, this might not hold to a broader grouping done in the original census.
For example the census under the label Asian groups together very distinct subgroups such as Indians and Pakistani into a larger whole.
In the same vein, the Black census category groups together Jamaican and Central-African communities.

#### ... And the Bad

Sadly, not all of our models performed as well.
Especially the model predicting ethnic make-up and, to a lesser extent, the model predicting area income.
*This is, unfortunately, part of the reality of our approach to this kind of analysis, but we might still learn something from this*.
Reasons for the ill performance could be multi-faceted.
One obvious reason is our grouping and labelling methodology and the underlying data collection in the census.
We already introduced the caveat that the terms used in the census data about ethnicity are especially broad, which might prevent the model from learning distinctions between the major groups as they are themselves made up of multiple smaller and distinct subgroups.
Another facet might be that for wealth, simply addressing household median income cannot easily reflect more complex interplays regarding wealth, especially in a rather expensive city as London.
There, home-ownership can play a leading role in the disparity between people with similar household incomes.
Adding to this that policies such as *[Right to Buy](https://en.wikipedia.org/wiki/Right_to_Buy)* can further tip the scale by granting long-time residents an economic advantage over later arrivals and short-time renters.
Thus the grouping conducted might not capture enough information to form more distinctive groups regarding economics, that can be differentiated by the model.
Something to note about the performance of both of these models is that they generally still achieved a high accuracy, as they were able to distinguish between the majority class and the minority classes.
However, they performed rather badly at distinguishing the minority classes between each other as can be seen in the confusion matrix for the classifier predicting majority ethnicity.
We can see that it is both rather *precise* (middle) and *recall*s the majority of the white majority areas (right).
But it is bad at identifying ethnically diverse areas, which get misclassified across the complete spectrum.

{% include Heatmap_Ethnicity_RandomForest_Nutrients.html %}

*After addressing the training of the models, we also want find out some more about their inner workings and what are the most impactful facts they used in their predictions.*

### Learning What the Model Says

Interpreting random forest models regarding their decision process is hard, as they present ensembles of tens to hundreds of decision trees learned in a way to increase variety in the decision process of the individual trees.
*Talk about not to see the wood for the trees*.
Consequently, the individual trees are already complex, and over the size of the ensemble, it is nearly incomprehensible.
However, to at least get a feeling for the underlying process we can use *feature importance*, the contribution of individual features, to the final model.
Analysing this for our two most accurate models, predicting religion based on nutrients and based on product consumption, we can make a few interesting observations.
What we need to keep in mind when doing this is that based on the training of the individual trees, the occurrence of a feature is at least to an extent influenced by random chance.
This is due to the fact that only subsets of features are considered for each split.
Thus an important feature might just have bad luck in the lottery and gets chosen a less often than other features.
Hence, feature importance needs to be taken with a grain of salt.

{% include FeatureImportance_Religion_RandomForest_Nutrients.html %}

For the model based on nutrients, we can see that especially nutrient diversity, protein, and salt consumption are often chosen splitting features in the classifiers.
Finding a rationale behind the importance of a feature can be difficult without knowledge of the way they split up the groups.
We might find some superficial explanations, such as vegetarianism being more prevalent in Hindu circles and thus leading to a lower protein, i.e meat consumption.
But finding in-depth explanations is very difficult.

{% include FeatureImportance_Religion_RandomForest_Products.html %}

Looking into which specific kinds of products have high importance regarding splitting presents a few more insights.
Especially fish, dairy, sweets, and wine consumption are highly important in identifying the religious majority in an area.
Once again, we might emit a hypothesis regarding a possible rationale behind their importances, i.e alcohol consumption is forbidden in conservative Islamic circles, milk products enjoy high approval in some Hindu circles, and so on.
While we can discover that these features are relevant in the final decision process, what we actually want to learn is *how* do they lead to the final decision we observe?
And especially, *In which way do they make groups distinguishable from each other?*

---

<a id='Responsible'></a>

## Can We Go Back?

To further improve on the performance of our classifier, we might now go with an even more complex model or refine our clustering approach.
Nevertheless, in this instance we might not opt for this option, and instead ask ourselves if we can go back.

A major concern with complex models such as random forests and boosting trees is that, while they perform well and are robust, they present a high level of obfuscation to the derivation of their final result.
For a human, they appear as a near-to-*black-box* that transforms an input into an output, without granting the individual understanding of its decision mechanism.
Addressing this concern as *Responsible Data Science* has become an increasingly important issue.
This is especially relevant when working with sensitive data concerning individuals and when performing societal analyses.
To do our part, additionally to our previous analysis based on random forests, we tried to train models that provide a better explainability to their result and thus get at least a cautious look into the black box.

<a id='Trees'></a>

### But I still like Trees

We start with *decision trees*, starting from the top root node the data is split based on different features into smaller groups that then again can be split further.
This allows for great explainability of their decision process.
Unfortunately, this also necessitates the constraint that the decision tree does not increase its depth or general complexity too much.
In our model training, we again used balanced accuracy scoring, grid search, oversampling and class-weights to boost model performance and inter-class fairness.
On the other hand, this also leads to getting far more complex and nested trees in comparison to a tree trained on accuracy.
To address the issue of complexity and overfitting we used a pruning method to remove smaller and indecisive splits of the tree classifier.
The resulting decision trees were unsurprisingly still rather complex.
But they provide better insights regarding their decision structure, with special interest put on the most decisive cuts between labels.
Many of the trees we trained performed worse than the random forest models for the same features and targets, however in many instances not to an extensive amount.
For example, in a more extreme case, the decision tree pendant to our best performing random forest model achieves a balanced accuracy of 0.927 on the test set, while the random forest model achieves a balanced accuracy of 0.93, hence constituting only a marginal improvement.
The random forest however also achieves a higher base accuracy and a better F1-score.
*Let's look into the decision tree model*.

{% include_relative images/Religion_Products.svg %}

First off, we can see that the first and most defined split is based on sweets consumption, with many majority Christian areas having a lower fraction of consumption.
For the next split, something interesting can be observed.
Splitting based on fish consumption separates the Muslim and Hindu majority communities almost perfectly.
As already hypothesised earlier, we can observe that alcohol consumption indeed is a splitting feature used to divide between Christian and Muslim majority communities.
Furthermore, our hypothesis about higher dairy consumption finds at least some confirmation based on a split.
Overall, using a decision tree model allows for a better exploration of the data, as we can now see the impact of certain splitting features on which we might now base further analysis and exploration.

---

<a id='AssociationRules'></a>

### *Rule Britannia*

An alternative when looking for explainable models is the use of *association rules*.
These derive rules in the form **antecedents ➞ consequents**, based on observations of common item sets.
In this instance, we used the different labels of the groupings as items. Additionally, we added especially high and low consumption of certain products or proportions of a nutrient as further items.

We used different measures used in association rule mining to evaluate the rules that we derived through the former item set design. However, given the way that item set and association rules are designed, we reach certain conflicts in using metrics, as they are also once again susceptible to the unbalanced distribution of labels.

In our analysis, we focus on rules of the form **nutrients/products ➞ socio-economic descriptors**. This means that we sought out rules that let us derive the socio-economic implications of nutrient distribution.

Before looking at some of the rules we found, it is important to note a few things. Firstly, we found *a lot* of rules, more than 7,000 actually with confidence above 0.6, distributed across 21 consequent sets. This means that there were 21 groups, i.e. distinct subsets of socio-economic descriptors for which we found antecedents.

Instead of going through these rules by hand, which wouldn't have led to anything anyway, we tried to find sensible ways of reducing the number of rules we consider. Looking at the groups for which we had obtained rules, we noticed that some of them seemed to be very similar. Thus, we computed the set of all antecedents for every subgroup and then used *Jaccard similarity* to determine how close the set of antecedents were between groups. We then kept, for every subset of groups with pairwise Jaccard similarity greater than 0.8 the best representative, that is, the rules for the group with the highest lift.

For example, instead of having the groups {Working Class, Secondary School} and {Working Class}, we only kept the first one as their respective antecedent sets had a Jaccard similarity of 0.94. To further reduce the selection of rules, we required every rule to have a confidence of at least 0.65 and a lift greater than 1.8.

This had some unintended consequences. We found after filtering that we had, in the process, lost all rules with support higher than 0.1. We then further observed there was a pretty strong dichotomy between rules. As a rule of thumb, a rule either had high support (>0.1) and on the other hand a low lift (< 1.2), or it had a high lift (> 2) and low support (<0.07). This comes down to the distinction between making general, rather weak statements about large populations and making very precise statements about small groups. In the end, we opted for the second approach, as rules with low lift simply don't make very strong statements.

From the remaining rules, we selected a few by hand, as putting them all here would probably triple the length of the page at the very least.

{% include AssociationRuleTable_Nutrients.html %}

These rules let us reinforce observations made beforehand.
For example, we can see that the working class tends to have increased spirits consumption while generally avoiding wine, or that the working class has a high sweets consumption.

Two very prominent rules on the Asian working class with extraordinarily high lift reinvigorate the hypothesis of lower meat consumption in that social group.

Sadly, the nature of the data did not allow us to obtain rules for other groups discussed above. For example, we did not find any rules about Hindu or Muslim minorities or concerning students, as these constitute small minorities compared to the other groups.
More surprisingly we did not find any rules concerning social strata other than the working class.
It is not surprising that we did not find rules for the upper class, as it makes up only a small percentage of society.
Nevertheless, we expected to find at least some rules for the two groups corresponding to the middle class.
A possible reason for why we didn't might be because these groups do not actually distinguish themselves that much through nutrient and product consumption.


### *Land's End*

Today we explored the interrelation of socio-economics and food consumption in the Greater London area.
While there were some obstacles in our way, we found some interesting tidbits and could make some interesting observations.
Sadly, not always did our approaches work and thus we tried to learn why they did not work out as planned.
Thus, this is a time for self-reflection and to learn what we could improve in future workd in exploring the interplay between socio-economics and food consumption.

#### To Improve the Recipe

After rounding up the analysis, we want to take a final look back and consider what can be done better and where we could extend further from the foundation we have built here.

As a next step, What we might consider is looking deeper into the data to find conjoint clustering and groupings based on a wider range of socio-economic attributes.
The most obvious example would be the differentiation between the major minorities in the UK, Indian, Pakistani, and Bangladeshi, which in our approach got clustered together based on the skewed singular ethnicity attribute.
An example to improve this would be based on adding majority religion to the clustering as further attributes.
Thus we could better differentiate between both groups.
However, this might also introduce further side-effects, which is why we kept it simpler for our initial analysis.
Additionally, we could take geo-distance into account, following the assumption that communities with a similar make-up are centralized into one or few clusters in a city.

Further efforts could be spent looking into data on a deeper granularity.
In our analysis, we focused on studying MSOA, as they provide high representativeness in the original Tesco data set, while also being more numerous than wards.
LSOA falls faster regarding representativeness, however could introduce new avenues to groupings, as smaller communities are not overshadowed by a larger area population.
Thus, this could allow to extend the original groupings and to address more socio-economic categories, single vs. [*DINK*](https://en.wikipedia.org/wiki/DINK) vs. family households, population density and dwelling type, social welfare recipients, age structure, which were sadly less differentiable on the MSOA level.
Concerning the models that we trained, more training data would especially help the random forest classifier.
While not solving the label imbalance, as it is to be expected with human populations, it could at least create more variety inside each category, which might be better than our approach of using synthetic data.

Last, but not least, concerning the model training we conducted, further refinement to the methodology we used could be applied to improve the model's performance.
For example, some consideration could be spent to use feature engineering, such as computing ratios between carbohydrates and sugars or fats and saturated fats.
However, training the best model possible was not our main goal, instead they helped us to better understand the data and to discover dividing lines with regard to food consumption.

<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia:

The Royal Family of *Windsor*, because of its German ancestry and its pre-WW1 *von Sachsen-Coburg und Gotha* name, celebrate Christmas on Christmas Eve, the 24th of December, not on the morning of the 25th as proper Englishmen would do.
On a related note, the royal connection to the *House of Hannover* brought over from Germany the tradition of Christmas trees and introduced it to the British Isles.

Until the Betting and Gaming Act of 1960, the only sport allowed to be played on Christmas Day was archery.
This was due to the Unlawful Games Act of 1541 which aimed to promote the use of the longbow rather than less publicly useful sports, such as football... Pardon, *soccer*.

After the 31st of December, Queen Elizabeth II will be the only person in the UK not to need a passport to enter the EU legally, as all UK passports are issued in her name.
This is the same reason why she does not require a driver's license to drive on British roads, even though she earned one for driving [a lorry during the Blitz](https://i.insider.com/5e8646eb1378e3116b2372a4?width=1100&format=jpeg&auto=webp).

*On Metatrivia*.
The word trivia is derived from the Latin words *tri* and *via*, with the former meaning *three* and the later *road*.
The Romans, big into public infrastructure, built many roads. Places, where three - or more - of these roads met, were convenient places to make public announcements and thus making the announcement common, i.e *trivial*, knowledge.

<a id='Conclusion'></a>

### Anything else?

Although it might date the creation of this data story a little bit, we wish you all Happy Holidays and a hopefully better 2021.
Take an example from the Londoners and have a nice increase in your fat, sugar and alcohol consumption while you are at it, and see you in 2021.
*If you read this in 2021, don't forget to cancel the New Years resolution GYM membership before it's too late.*

{% include year_plot_nutrients.html %}


