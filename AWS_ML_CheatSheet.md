# Sagemaker 

## Exploratory Data Analysis

### Imputation Techniques

#### Imputation using Deep Learning
Using a library such as **datawig** python library, a deep leanring approach used deep neural networks to impute missing data values. This is the most accurate strategy at imputing **categorical** values.




## Data Engineering

1. The Amazon Kinesis Data Streams API PutRecords call is the best choice for processing in real-time since it sends its data synchronously and doesnt not have the processing delay of the producer library. Therefore, it is better suited to real-time applications.

    1. Adding Multiple Records with PutRecords
        **Request Syntax**
        ```
        {
            "Records": [ 
                { 
                    "Data": blob,
                    "ExplicitHashKey": "string",
                    "PartitionKey": "string"
                }
            ],
            "StreamName": "string"
        }
        ```

    2. Adding a Single Record with PutRecord
        **Request Syntax**
        ```
        {
            "Data": blob,
            "ExplicitHashKey": "string",
            "PartitionKey": "string",
            "SequenceNumberForOrdering": "string",
            "StreamName": "string"
        }
        ```


### UseCase Examples

1. **Kinesis Data Streams and Kinesis Data Analytics** - _USECASE_ - USe Amazon Kinesis Data Streams to capture the streaming data. Use Amazon Kinesis Data Analytics to clean, organize and transform the data and then output the data to the S3 data lake using a Lambda fucntion.

2. **Kinesis Data Streams, Kinesis Data Analytics & Kinesis Firehose**

    <image src="./images/KDS.png">

3. **Kinesis Video Streams** - _USECASE_ - Use Amazon Kinesis Video Streams to stream the video to a set of processing workers running in ECS Fargate. The workers send the video data to your SageMaker machine leaning model which identifies alert situations. These alerts are processed by Kinesis Data Streams which uses a lambda function to trigger alert system.

## Modeling

### Supervised Algorithms


#### Factorization Machines

Sparse Data

Classification or Regression

Find factors we can use to predict a classification (click or not? purchase or not?) or value (precited rating) given a matrix representing some pair of things (users and items)

**Usecases**
1. Recommender System
2. Click Prediction

**Training Data Format**
recordIO-protobuf with float32 (NO CSV)

**Instance Types**
CPU (recommended) or GPU (dense data)


### Unsupervised Algorithms

#### Random Cut Forest
 Random Forest Algoritm is well known to increase the prediction accuracy and prevent overfitting that occurs with a single decision tree.

#### Neural Topic Model (NTM)

#### Latent Dirichlet Application (LDA)

#### Principal Component Analysis (PCA)

Dimensionality Reduction

Covariance matrix is created then Singular Value Decomposition (SVD)

**Two modes -**
1. Regular - For Sparse data for moderate number of obeservations and features
2. Randomized - For large number of obeservations and features, Uses approximation Algorithm

**Important Hyperparameter**
1. Algorithm_mode
2. Subtract mean

**Instance Types**
CPU or GPU


#### IP Insights
IP address usage pattern
uses Neural Network under the hood

**Training Data Format**
csv

#### K-Means Clustering


### Forecasting Algorithms

1. Arima
2. Prophet
3. DeepAR
4. DeepAR+


#### Sagemaker Modelling

1. 4 options for Model Creations:
    1. SagemakerConsole
    2. Jupyter
    3. SageMakerSDK
    4. Apache Spark

2. Most sagemaker algorithm accept csv
    1. For unspervised ML, we need to specify the absence of a label (the absence of a target value we are trying to predict)
        ```Content-Type = text/csv;label_size=0```
    2. The Target value should be in the first column with no header.

3. For optimal (fast) performance, the **optimzed protobuf recordio** format is recommended.

4. **For Training** - _CreateTrainingJob_ API (High-Lvel Python Library)

5. 2 jobs - 
    - training (potentially on large GPU)
    - model API call (deploy to prod on a less CPU expensive)

6. Amazon has an Elastic Container Repository that has containers for each algorithms.

    <image src="./images/ECR.png">

7. Amazon fetch the proper image and spin up inside Sagemaker space. Then the job can access the data on S3.

    <image src="./images/ECR_train.png">

8. We can also specify our own docker file. API Gateway allow us to use REST calls on our hosted program.

    <image src="./images/ECR_Docker_Train.png">
    <image src="./images/ECR_Docker_Host.png">

9. Training with Apache Spark

    <image src="./images/Sagemaker_Spark.png">

10. **:1 vs :latest container tags** Algorithms that are parallelizable can be deployed on multiple compute instances for distributed training. For the Training image and Inference Image Registry Path column, use the :1 version tag to ensure that you are using a stable version of the algorithm. You can reliably host a model trained using an image with the :1 tag on an inference image that has the :1 tag. Using the :latest tag in the registry path provides you with the most up-to-date version of the algorithm, but might cause problems with backward compatibility. Avoid using the :latest tag for production purposes.
    



##### Mechanical Turk
provide an interface to farm out tasks for human beings to complete




### Modelling Metrics 

#### Classification
1. **ROC Curve** - Useful when both outcomes have equal importance.
2. **Precision** - it only takes into account the percentage of positive cases out of the total predicted positive.
3. **Recall** - it only takes into account the percentage of positive cases out of the toal actual positive.
4. **PR Curve** - PR curve is best suited to evaluate models on data sets wgere most of the cases are **negative**. For example - Cancel screening dataset.

##### Confusion Matrix

<image src="./images/Confusion_Matrix.png">

Precision = TP / (TP + FP)  or True Positive / Predicted Results

Recall = TP / (TP + FN) or True Positive / Actual Results

1. Precision is how certain you are of your true positives. Recall is how certain you are that you are not missing any positives.
2. Choose **Recall** if the occurence of the **false negatives** is unaccepted/intolerable. For example in the case of diabetes that you would rather have some extra false positives (false alarams) over saving some false negatives.
3. Choose **Precision** if you want to be more confident of your positives. For example, in case of spam emails, you would rather have spam emails in your inbox rahter than some regular emails in your spam box.





### Reguralization Techniques -

1. Dropout (Used in Neural Networks only)
2. Early Stopping
3. **Lasso (L1) and Ridge(L2) Regression** - Lasso regression penalizes weights from non-important features to 0 as we use |w| in the cost function. Lasso Regression helps in automatic feature selection. Ridge Regression (L2) penalizes the weith but not until 0. Ridge regression adds |w^2| to the cost function.




### Other Sagemaker Services

#### AWS Comprehend

NLP and Text Analytics

Used to extract sentiment

#### Amazon Translate 

#### Amazon Transcribe
Speech to Text

#### Amazon Polly
Text to Speech


## ML Implementation and Operations

### Elastic Inference 
Elastic Inference is used to speed up the throughput of retrieving real-time inferences from models deployed as SageMaker hosted models. Elastic Inference accelerators accelerate your inference calls; they aren't used while training.