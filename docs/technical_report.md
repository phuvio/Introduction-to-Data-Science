# Unveiling the hidden potential of Helsinki: Harnessing green spaces for a thriving city
## Greenspaces and residential segregation in Helsinki 
Jenni Liikanen, Ossi Inkiläinen, Petteri Huvio

The target of our project was to analyse whether the amount and distribution of green areas within Helsinki could have an effect on social segregation. Green areas are found to have a positive effect on well-being, whether it is physical, mental or social[^1]. Therefore it is important that people would have access to green areas in a manner that serves their needs, and that social differences would not hinder this. 

As a base unit, we observed Helsinki divided into city’s defined sub-districts. We did not observe such special districts as Aluemeri (sea) and Santahamina (military garrison). We investigated what kind of people live in each sub-district and how much green space there is. 

The intention was that our results could be of aid for different stakeholders in urban development, but mainly for municipal decision makers. They could be seen as a tool for zoning and land use planning.

## Data
On this project, we used purely open data provided by the city of Helsinki. Usage of just open data makes it possible for anyone to easily reproduce or critique our results, without the need to request some special data, or for us to have greater concerns, for example on  privacy.

### Data collection
We collected data for our project mainly from three sources: Aluesarjat (“regional data series”) statistical database from Greater Helsinki Open Statistical Databases, Helsinki Region Infoshare open data service and Helsinki map service. We used the regional data series and the Helsinki Region Infoshare data to collect data about the people and housing in Helsinki. The Helsinki map service was used to collect data about green areas in Helsinki and to get the geospatial coordinates for the districts of Helsinki. All data was from the years 2021 or 2022.

Greater Helsinki Open Statistical Databases have a user interface through which one can select and download data simply through the web in tabular form. Helsinki Region Infoshare offers some datasets both as downloadable files and to be used via API. However, the data that we collected from there did not have a proper data structure and the API provided only read the same xlsx file that was downloadable from the service. We decided to download the data files first and process the data from them. 

For the Helsinki Map Service, we used the WFS interface provided by the service, to download data on district areas, green areas and built up areas within the city of Helsinki. Map data was fairly consistent between subjects.
### Preprocessing
#### Population data
We collected data about the population living in different regions in Helsinki. We wanted to find out what kind of people live in each region and how they live. Our aim was to find information about the demographics in each region. We found the following data that seemed relevant for the topic: 
- Level of education
- Main activity: employed, unemployed, students, pensioners etc.
- Income and taxation per person and per household
- Families by family type
- One-person households 
- Morbidity indices: mortality rate, share of people on disability pensions among the working-age population, share of people entitled to special long-term medicine reimbursements and chronic diseases index

Some of the data was provided as the number of people per feature in the region. As the regions have different amounts of residents, it is difficult to make comparisons based on numbers. Therefore, in these cases, we converted the exact numbers to proportional shares of people. 

The data was mostly offered per gender and we used this division in our analysis. If the data was not provided on gender-level, we generalised the total number to both genders.

Our target was to analyse the sub-district level data but some information was only available on district level. In those cases we generalised the district-level data to each sub-district within a district. 

In the preprocessing phase we processed the data in Jupyter notebooks. All data and notebooks were saved in and shared via a GitHub repository. We found working with the same notebook difficult as group members had different operating environments on their computers. Thus, we ended up using our own notebooks for data handling but sharing all the information in them to others. We also created separate notebooks for preprocessing and for analysing purposes. 

The data from different sources had different structures, and our preprocessing started with formatting all the data into a similar form and the same sub-district level. Columns were named in a way that retained the origin of the data and described the data more specifically, for example “Education % 25-44 years Lowest higher and lower university level”. We dropped unnecessary data. For example, some data included major-district level, which we did not use in our analysis. Categorisation was made for genders. Some demographic data was missing for some sub-districts with too few people, for example if there were not enough residents for each age group. In these cases district-level data was used. This way we did not lose valuable information. 

#### Spatial data
From the map data, we calculated the areas of every type of land use per sub-district. Data was calculated using GIS software, in our case QGIS. For the data, an overlap analysis of different areas was done. This required some data processing, such as geometry correction and elevation fixes. We also did a simple exclusion of sea areas from the land masses. The data calculated in this manner was then combined with people data.

### Analysis
We decided to use linear regression. The idea was to first select the variables that most predicted the size of the green areas in the districts. After that we would have reversed the linear regression so that the size of the green area would have been a predicting variable. Then, we could have created a model where by changing the size of the district’s green area, we could have predicted the change in population. At least for the selected variables.

Firstly the demographic data was merged with the green spaces data at the sub-district level.  At this point we had a dataframe with 368 rows and 44 columns. Next step was to check the multicollinearity of different variables. Analysis showed that there were many groups of variables with multicollinearity problems. We analysed these variables and removed some of them to solve the multicollinearity problem. After that we had 31 columns in our dataframe.

Then we used statsmodels linear regression function and performed linear regression on all remaining variables. No regression was found. Feature selection was needed. For that we used scipy’s zscore function, which gave each variable a score. There were 8 variables with elevated score and we selected them for the following linear regression. It gave 7 % predictability. So the result was not satisfactory, but a correlation with green areas was found for some variables. 

After a discussion with the instructor we tried a new approach. In the original data, several variables were distributed by e.g. age. We added them together and used the summed variables in linear regression. There were still some multicollinearity problems with some variables, so we still had to remove some variables. In the end we had 23 variables. Next we did feature selection again and ended up with 6 different variables. This approach did not do any better. There was only 2 % predictability. 

Finally we used cross-validation to see if that could help. We used Scikit-learn’s KFold, cross_val_score and RFE functions to do the cross-validation and linear regression. That gave the same 2 % predictability, so no improvement.

## Results
Our original idea did not work. Therefore, we were not able to build a model that could have predicted changes in the district's population with changes in the size of the green area. Instead we did find out that there were variables that correlated with the size of the green area. One important point was that the green areas had not been divided more precisely. Probably, if it had been possible to separate parks, fields, forests etc. in the data, linear regression would have produced better results. This is because the parks are more concentrated in the centre of Helsinki, while the forests and especially the fields are on the outskirts of the city. And then the original idea would have produced results.

Although we were not able to produce a working model of the subject, we communicate our findings on a website at [https://phuvio.github.io/Introduction-to-Data-Science][def]. 

## Future steps
Looking at the data on the distribution of green spaces between the districts of Helsinki, it is visible that the amount of green areas differs between districts. Sometimes the reason for this seems clear: in the city centre there are more built-up areas and usually less space for green, whereas in the suburbs there is often more space between houses and therefore perhaps more room for green areas. But it remains an open and an interesting question whether social matters have an effect on how much green space there is in the area, and if this information could be used to help distribute welfare between different groups of people. 

While our study did not produce a model that could be used, we suggest that the topic would be further investigated. It might be useful to have more districts in the data, and therefore we suggest including other larger cities in the study. These could be, for example, the whole Helsinki metropolitan area, i.e. Espoo, Vantaa and Kauniainen in addition to Helsinki, and Tampere, Oulu, Turku, Jyväskylä and Kuopio. Having more districts might make it possible to find more similarities between people living in the same kind of environments, and this could help in building a model between the social matters and green areas. 

Another issue to investigate deeper is the more refined data about green areas, as was suggested in the results section. Our data only included the green areas that are marked as such in the zoning of the city. Separating parks, fields and forests and adding yards and gardens might make it possible to find more connections between the types of people and the green areas around them. 

## Learning outcomes and conclusions
During the project we noticed that our data did not provide us with results in an expected way. We did not give up but tried another approach. This did not give us better results, though. Nonetheless, we are happy that we tried, and we think that this subject is important, even if we did not manage to build a usable tool around it. 


We learned about many aspects of data science while working with the project, for example about collecting and processing of data, making exploratory analysis with the data, choosing methods that work with the data that we had, and analysing the results of the chosen methods. We noticed, especially, that it can be difficult to find the right visualisation for each phenomenon, to best highlight the issues that are important for the viewer to see about the matter. 

While we were not able to make our original idea work, not finding a proof to a hypothesis can also be a result. However, we think that the subject could do with more research, and perhaps a predicting model could be found with more data. 

[^1]: N. Finel, “Lähivihreän hyvinvointivaikutukset – tietopaketti,” Ympäristö ja Terveys, no. 1, 2022. Available: [http://urn.fi/URN:NBN:fi-fe2022040426974][def2]. 

[def]: https://phuvio.github.io/Introduction-to-Data-Science
[def2]: http://urn.fi/URN:NBN:fi-fe2022040426974