# Forest-Cover-Prediction

### Problem Statement 

To build a classification methodology to predict the type of forest cover based on the given training data.  

#### Language Used - Python

#### Deployed on - Heroku

#### Framework - Flask

#### Tools - PyCharm

##### Algorihtms - K-Mean Clustring, XG Boost Classifer and Navis Bayes Classifer

#### Accuracy Metric - AUC Score


### Architecture

![Architeture](https://user-images.githubusercontent.com/66250589/116099567-b7f73180-a6c9-11eb-879d-3eafcacba085.PNG)


### Data Description 

The client will send data in multiple sets of files in batches at a given location. Data will contain different indicators to classify them betwee given types of forest cover. 

### Data description: 



| Name | Data Type | Measurement | Description | 
|----- | ------- | ----------- | -------------- |
| Elevation | quantitative | meters | Elevation in meters |
| Aspect | quantitative | azimuth | Aspect in degrees azimuth | 
| Slope | quantitative | degrees | Slope in degrees | 
| Horizontal_Distance_To_Hydrology | quantitative |  meters | Horz Dist to nearest surface water features | 
| Vertical_Distance_To_Hydrology | quantitative | meters | Vert Dist to nearest surface water features |
| Horizontal_Distance_To_Roadways | quantitative | meters | Horz Dist to nearest roadway |
| Horizontal_Distance_To_Fire_Points | quantitative | meters | Horz Dist to nearest wildfire ignition points |
| Wilderness_Area (4 binary columns) | qualitative | 0 (absence) or 1 (presence) | Wilderness area designation |
| Soil_Type (40 binary columns) | qualitative | 0 (absence) or 1 (presence) | Soil Type designation |
| Cover_Type (7 types) | integer | 1 to 7 | Forest Cover Type designation. |

 

Apart from training files, we also require a "schema" file from the client, which contains all the relevant information about the training files such as: 

Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns, and their datatype. 


### Data Validation

In this step, we perform different sets of validation on the given set of training files.

#### Name Validation-

We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation. 

After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per 

requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

#### Number of Columns - 
We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."

#### Name of Columns - 

The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

#### The datatype of columns - 

The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to 

"Bad_Data_Folder".

#### Null values in columns - 

If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

### Data Insertion in Database

Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database.

#### Table creation in the database -

Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the 

table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training 

files.

#### Insertion of files in the table - 

All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and 

is moved to "Bad_Data_Folder".

### Model Training

1: Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.

2: Data Preprocessing    

   a) Check for null values in the columns. If present, impute the null values using the KNN imputer. 

   b) Encode the categorical values in the class column. 

   c) scale the numerical values in the given dataset after we split them for test and train. 

3: Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic 

selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms .

 To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction. 

4: Model Selection - After clusters are created, we find the best model for each cluster. We are using two algorithms, "Random Forest" and "XGBoost". For each cluster, both the 

algorithms are passed with the best parameters derived from GridSearch. 

### Prediction

Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.

1: Data Preprocessing

a) Check for null values in the columns. If present, impute the null values using the Categorical imputer.

b) Check if any column has zero standard deviation, remove such columns as we did in training.

2: Clustering -

KMeans model created during training is loaded, and clusters for the preprocessed prediction data is predicted.

3: Prediction -

Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.

Once the prediction is made for all the clusters, the predictions are saved in a CSV file at a given location and the location is returned to the client.

### Deployment

We will be deploying the model to the Heroku Cloud platform.

### Advantage of Heroku:

1: Beginner and start-up-friendly

2: It allows you to create a new server in just 10 seconds by using the interface of Heroku Command Line.

3: This cloud computing platform takes care of patching systems and keeping everything healthy.

4: A range of automated functionalities including the scaling, configuration, setup, and others

5: Easy integration with other AWS products.

### Output:

After compilation you can see this type of interface. Where you can give Custom files path for prediction or Default File prediction.

