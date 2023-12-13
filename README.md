# SpaceX-Falcon-9-First-Stage-Landing-Prediction

![background pix](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/69e31b87-a43a-4386-a509-78389d3276cf)

# Introduction
Companies are making space travel affordable for everyone. Virgin Galactic is providing suborbital spaceflights. Blue Origin manufactures sub-orbital and orbital reusable rockets. Spaces X’s Falcon 9 launch like regular rockets. Perhaps the most successful is SpaceX. One reason SpaceX can do this is the rocket launches are relatively inexpensive. SpaceX advertises Falcon 9 rocket launches on its website with a cost of 62 million dollars. other providers cost upwards of 165 million dollars each, much of the savings is because SpaceX can reuse the first stage.
Therefore, the aim of this project is to determine if the first stage will land with the aid of machine learning. The objectives of this task are:
- To determine if the first stage will land will land Successfully
- To determine the price of each launch.
- To determine the most important features that will determine a successful landing.
# Data Collection
- The data was collected from web and SpaceX REST API
- Get request was performed using the requests library to obtain the launch data.
- The response data was in the form of json object was converted to a dataframe and normalized.
- Data was also obtained from Wikipedia using webscraping
- Columns and variable names were extracted from the HTML table header.
- These data were presented in dataframes
- The data was cleaned and presented in the desired format
# Data Wrangling
- Import the libraries and load the SpaceX dataset
- Calculate the number of launches on each site
- Calculate the number and occurrence of  each orbit
- Calculate the number and occurrence of mission output per orbit type
- Create a landing outcome label from outcome column
# EDA with Data Visualization
Visual Exploratory data analysis was performed to better understand the trends of the features and to establish the existing relationships in the dataset. Catplot was used in order to visualize the relationship between flight number and launch site. We discovered that no rocket launched for heavy payload mass greater than 10000. The plot is shown below.

![cat plot](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/0ff5de43-f44c-47c7-8d3b-be921bdf52fb)

Bar chart was used to visualize the relationship between success rate of each orbit. The plot is displayed below.

![bar plot](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/602eedbb-5659-417d-b7a9-ca24c84d6083)

Scatter plots were used to visualize the relationship between flight number and orbit type, payload and orbit type. This can be seen in the plot below.

 ![scatter plot](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/b73031d0-4307-48f9-9c44-47bbaf1043a6)

Line plot was used to visualize the success yearly trend as show below.

![line plot](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/7e60d003-684e-47df-b45e-944af65fff10)

# EDA with SQL
To gain more insights, the following SQL queries performed
#### Names of the unique launch site in the space mission
```
%%sql 
SELECT DISTINCT(LAUNCH_SITE) FROM SPACEXTBL;
```

![Names of unique launch sites](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/1c7dd544-0d9b-4602-b022-8c7dd4b266df)

#### Five launch sites that begin with the string CCA
```
%%sql
SELECT *
FROM SPACEXTBL
WHERE Launch_Site like 'CCA%'
LIMIT 5
```

![launch site begins with CCA](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/ea87b5bc-516f-41fc-beaa-d9c926810b84)

#### Total payload mass carried by boosters launched by NASA (CRS)
 ```
%%sql
SELECT SUM(PAYLOAD_MASS__KG_)
  FROM SPACEXTBL
WHERE customer = 'NASA (CRS)'
```

![Total payload mass](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/e2aef373-dbf6-4d62-8cc2-00da30b5604d)

#### Average payload mass carried by booster version F9 v1.1
```
%%sql
SELECT AVG(PAYLOAD_MASS__KG_)
  FROM SPACEXTBL
WHERE Booster_Version like '%F9 v1.1%'
```

![AVG payload mass](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/c861d8a9-aed0-44fc-a7aa-2bb3c88aa0bc)

#### Date when the first successful landing outcome in ground pad was achieved
```
%%sql
SELECT MIN(DATE)
FROM SPACEXTBL
WHERE Landing_Outcome = 'Success (ground pad)';
```

![first successful landing](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/1ebf4f37-b808-47fb-9021-b251530de238)

#### Total number of successful and failure mission outcomes
```
%%sql
SELECT count(Landing_Outcome) AS Success_Outcome,
       (SELECT count(Landing_Outcome)
          FROM SPACEXTBL
         WHERE Landing_Outcome like '%Failure%') AS Failed_Outcome
  FROM SPACEXTBL
 WHERE Landing_Outcome like '%Success%'
```

![Total no of success and failures](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/7c46291c-f766-478a-8c6f-8171f5a116c0)

#### Rank of successful landing outcomes
```
%%sql 
SELECT Landing_Outcome, Count(Landing_Outcome) AS Total,
       Rank () OVER(Order by Count(Landing_Outcome) DESC) AS Success_Landing_Rank
  FROM SPACEXTBL
WHERE Date > '2010-06-04' & Date <= '2017-03-20'
   AND Landing_Outcome like '%Success%'
 GROUP BY Landing_Outcome
 ORDER BY Total DESC
```

![rank](https://github.com/DannyRukks/SpaceX-Falcon-9-First-Stage-Landing-Prediction/assets/97890440/e9602eaa-7b21-4b82-8307-d491200877d4)

# Predictive Analysis (Classification)
We built a machine learning a machine learning pipeline to predict if the first stage of the falcon 9 will land successfully. This begins with preprocessing of the dataset. This was followed by standardizing the dataset. The dataset was split into training and testing dataset. Models such as Logistic regression, Support Vector Machines, Decision tree and K-Nearest neighbours were used to train and test the data. GridSearch was performed allowing us to find the best hyper parameters that allow a given model to perform best. Visualizing the built model accuracy for all built classification models in a bar chart shows that decision tree has the highest classification accuracy with an accuracy of 88.9%. The confusion matrix is shown below:



From the confusion matrix shown above the model correctly predicted 5 outcome of 6 not landing and correctly predicted 10  outcome landing from a possible 12.
# Conclusions
The Decision tree classifier has been proved to be the best model to be used for the prediction (classification) as it has the highest classification accuracy. The decision  tree classifier has an accuracy of 88.9%. The model correctly predicted 5 outcome of 6 not landing and correctly predicted 10  outcome landing from a possible 12. The launch sites are located near highways, coastlines and railways for ease of transportations. The trend analysis shows that there has been record of increasing success rate since 2013 till 2020. The price of each launch can now be determined

