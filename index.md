### Where we are

London, England, Captial of an old Empire that sailed forth to conquere the world in search of tea and spices only to return to a diet of <a href="http://www.foodsofengland.co.uk/peawet.htm">pea wet</a> and beer. Or does it? As a consequence of its history, London today is one of the most multicultural cities in Europe and consequenty also one of the most multiculinary ones. This allows us to analyse data about eating habits of modern Londoners to learn what on our plates divide us and what defines us. 

In the following we want to analyse a data set of the Tesco Purchases of Tesco Loyality Card holders in districts of the Greater London metropolitain area, and combine this with information about the areas inhabitants collected by the Greater London census.  

Tesco is the largest overall supermarket chain and thus also the largest out of the four domestic chains in the UK. It has a wide spread all over the Greater London metropolitian area as can be seen below. 

{% include Store_Locations_London.html %}

Our goal is to learn about the interplay of food consumption and socio-economic circumstances and to be possibly predict socio-economic facts about an area based on knowledge of its consumption habits. 


To give a rough outline, we start of with a <a href='#PreCurs'>precursoary look </a>into the data we have collected, and also look into the question: <a href='#WhoIsTesco'>"Who is a Tesco Customer"</a>. Following this, as a foundation for later analysis and to help with the understanding of the data we want to cluster areas based on socio-economic factors and look into their interplay. This also allows us to assign discrete inforamtion for later analysis. 

### Why it is important

Why is food important, sounds like an easy question to answer, however besides the importance of having sufficent food, the modern day issue in developed countries is having a proper composition and balance of nutirents. An improper diet might not lead to inbalanced <a href="https://en.wikipedia.org/wiki/Humorism">humors</a>, as ancient doctors believed, but has far more intertwined effect on the individuals health. 

This becomes also important as an issue of public health, as the UK as most other European Countries uses a socalised healthcare system, in which individual well-being and preventable health-outcomes become a <a href="https://www.bmj.com/content/349/bmj.g5143">societal concern</a>. 

### What the Data says

The original Tesco data set was created based on purchase histories of Tesco Loyality Card holders, that were then aggreagted based on Place of Residency of the card holder to different census areas in the Greater London area. The data set, while enormus with 411 million individual purchased items, does not allow to attribute the purchases to individuals or groups of individuals, i.e families. Thus instead the researchers opted for tracking consumption habits in the different areas based on an average area product, an abstraction of nutirent content for each product that is consumed in the area.   

The Cesus data we process is provided by the London Data Stores and covers the Data from the most recent Census, 2011, which might seem far off, however is close to the collection year of the Tesco Data in 2015.  

<a id='PreCurs'></a>

### What we can learn from the Data

We start of with some precusory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights.


An intersting insight can be won when looking into an areas alcohol consumption in relation to its inhabitans wealth and ethnic make up. A relationship that, not to spoil, but rather to cock Chekov's gun, can be observed in multiple models we used in analysis. 

{% include 3D_Plot_White_Median_Income_Alcohol.html %}

Interesting, especially in this context is that, if we divide the data up into the consumption of specific types of products consumed in an area, thus into Wine, Beer and Spirits. We can see the summit line previously observed only to re-occur in regard to Wine consumption, while the pattern is nearly inverted for consumption of Spirits and gets roughed up for the consumption of Beer. 

{% include 3D_Multi_Plot_Test.html %}


<a id='WhoIsTesco'></a>

#### Who shops at Tesco

To answer this question we looked first into an important information that the researches provided with the dataset, the relative representativness that each area achieved. **Representativness** here represents the number of loyality card holding customers vs. the number of area residence. An area with a low representativness has few Tesco customers and thus prevents extrapolation, as the few Tesco Customers would then speak for the majority of Residence in an Area. *The similarity to the UKs first-past the post election system is left uncommented by the Author*. This however also allows us to look into the interrelationships that cause an area to have a high representativness and thus many Tesco Customers. Looking into the spearman correlation, statistically significant correlation between representativness and socio-economic factors can be observed in the following:     

{% include Correlation_Representativness_Norm_White.html %}

These results can bring an interesting explaination to results intially discovered by the orginal data set reseachers, which discovered a decreasing representativness and number of transactions in areas of Southern London. Especially areas going closer to the Home Counties. These areas are compromised of the old white middle class leaving the urban core of London in the wake of Suburbaniziation during the Post-War years, which was later followed by middle class migraint communities. 

{% 
include Map_Factory_Representativness.html
%}


As an alternative hypothesis this can also be attributed, at least in part, to skewed data collection, as in the original data set a higher number of Tesco Stores from which the Data was collected were situated north of the Thames.

### The who is who of London

As an important first step to explore the population of London, we need to subdivide the population of London into ares with high similarity in regard to socio-economic make-up. To achieve this, we conducted Agglomorative Hierachical Clustering, to identify most similar subgrouping of the population based on attributes, such as Education, Median Household Income, Ethnicity and Religion. We identified a number of groupings, i.e clusters, for the different attributes based on silhoutte score.   

{% include ethnicity_clustering.html %}

#### Giving the Child a Name

The Groupings, however, only provide information about similarity between areas not direct labels for each grouping and what individual factors make them similar. Thus we needed to conduct our own further analysis to identify this information. 

As the weather in London is rather Covid-ty around this time of the year and thus can't validate our findings in Person, we conduced our study of the inhabitans in the subgroubed areas, based both on information present in the data and verified our conclusion, with wider research on the areas. 

For the grouped areas we identified labels based on the shared attributes. One example case, was the groupings based on the education level of the areas population, which resulted in the model reaching a high silhoutte score for 3 clusters. For these three clusters we then analyzed the areas median number of inhabitants with a certain education level tracked in the census. For the first Grouping we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education. For the second Grouping we can see that these include the Areas with an exceptionally high student population, which we can then further confirm, as these areas are those directly located around the major univerisities in London.    

{% include education_clustering.html %}

As an addional source for confirming our Clustering and to utilize in later analysis we reference an Grouping conducted for policy makers in the Greater London area, based on K-Means, which provides groupings based on a wider range of joint-attributes and deeper analysis of socio-economic trends in the UK.

As a final look into the groupings conducted by us, we can look into the realtive frequency of area labels. 

{% include Donuts_Clusters.html %}

Swine pork belly consectetur, ut reprehenderit nisi brisket ea. Pork chop reprehenderit irure esse. Nisi sirloin laborum aliquip pork voluptate officia in cupidatat. Ipsum irure enim prosciutto. Aute porchetta t-bone, in sausage enim ut chislic venison laboris eu lorem.


Boudin reprehenderit tail, shankle cillum landjaeger shank eu pastrami. Salami ut magna occaecat deserunt, fatback pancetta picanha. Anim in dolore mollit voluptate excepteur salami proident pork loin. Culpa strip steak ham hock ad hamburger nisi sirloin salami capicola picanha chislic. Nulla spare ribs kielbasa dolore sausage ad est quis swine picanha.


### Can we go deeper?

For our 

Based on the 


### Can we go back?


{% include AssociationRuleTable_Nutrients.html %}


#### But I like Trees 

{% include_relative images/treeviz.svg %} 


### Anything else? 




