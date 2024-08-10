## Name: Valentino Ong
## NRIC: G0644902L

# Pet-Adoption-Prediction

## Overview of submitted folder and folder structure:
The submitted folder consist of a eda.ipynb file where EDA is performed in the form of data visualisation, statistical methods etc as well as model evaluation is also inside this file. Inside the src folder, it contains a dataprep.py where data is pre processed and split into into the training and testing, then place into separate csv files where it will be then used in model.py for input as training and model evaluation. The folder structure is as what is requested.

## Programming Language, run environment prerequisite & list of libraries/packges used in EDA and pipeline
**Programming Language**: Python 3.9.18
**OS platform & Version**: Microsoft Windows 10 Education, 10.0.19045 N/A Build 19045
**Libaries: nltk==3.8.1**, pandas==2.1.4, scikit-learn==1.3.0, numpy==1.19.3, seaborn 0.12.2, matplotlib 3.8.0, xgboost: 2.0.3
**Tools**: Anaconda, VS Code

## Overview of Key findings:
There are a total of 49 features and 14993 rows of data that I can use. From the EDA carried out, There are missing values in Name,Description and BreedName. It is actually quite difficult to properly see the relationship between the various features as the relationship can be very random at times. After the EDA and various testing through checking model accuracy, I decide to drop PetID, Name, RescuerID, AgeBins, FeeBins, BreedBins,StateBins,VideoAmtBins, PhotoAmtBins,QuantityBins,Adopted,TypeName,GenderName, MaturitySizeName, FurLengthName,VaccinatedName, DewormedName, SterilizedName, BreedName,BreedBinsName,StateName,StateBinsName,ColorName,AdoptedName as they have zero to little effect in helping to classify the AdoptionSpeed. For the Description, I created a DescriptionLength and DescriptionWordCount which serve to be quite useful as there seem to be a relationship between these features and AdoptionSpeed and then from the Description, created a tf-idf out of it for the top 1000 words to use for training the model. I was able to find out tf-idf to be useful as I compared including and excluding tf-idf to train the model, and including tf-idf gives a higher accuracy which can be found in the EDA.ipynb. In conclusion, I excluded all the features i mentioned above and in addition to what if left, used DescriptionLength, DescriptionWordCount and tf-idf of the top 1000 words to train the model.

## Instruction for executing pipeline and modifying parameters
Before executing the pipeline, first go to dataprep.py where data pre processing occurs. Ensure that all the libraries are installed first. In the main() function or line 118 specifically, change the path of the csv file that want to be input. In line 135-138, the path can also be modified to place the X_train,X_test,y_train,y_test csv files into the path you liked but make sure to also change the path in model.py line 9-13 under load_data() function as the csv files will be used to train the model here. Similarly, if you want to change where the model is saved, can change line 44 and 45 in model.py for the random forest classfier model and in line 88-89  for the vectorizer model in dataprep.py also. You can also change the GridSearch Paramters in model.py line 31-35 to a more complex one but I couldnt as it took too long to run and my device could not handle it. If everything is fine, can just run it through the run.sh. 

## Logical steps/flows in the pipeline
In the pipeline, it execute the dataprep.py first. In dataprep.py, first the data is loaded from a csv file, and then new features are created which are DescriptionWordCount, DescriptionLength and DescriptionCleaned where DescriptionCleaned is derived from the feature Description undergoing punctuation cleaning, keeping only alpha, removing of stopwords and stemming to the base form. After the features are created, the features that I want to include into the model training is extracted and label encoding is also performed for non integer features. After this, the DescriptionCleaned feature is vectorized and converted into tf-idf of the top 1000 words using sklearn TfidfVectorizer. After the tf-idf is created, it is then appended back to the other feature columns where they are then split into training and testing data with test size being 20%. After this, they are convert into CSV files which will be used to train the model in model.py. Now the next stage, in model.py, a randomForestClassifier is initialized and using GridSearchCV, will try to find the best parameters for the classifiers. After finding the best parameters and fitting the training data, the model will predict the X_test data and the performance is evaluated using classification report.

## Explanation of model choice:
I decide to use Random Forest Classifier as among the few baseline Models I tested, **Random Forest Classifier**, **Gradient Boosting Classifier**, **AdaBoost Classifier**, **SVM**, **XGB Classifier** and **Gaussian Naives Bayes**, the Random Forest Classifier produce the highest accuracy of **0.42**. This can be found in the eda.ipynb file. 

## Evaluation of models and metrics explained
So the model used is random forest classifier. After finding the features I want to use, I use GridSearchCV to try to find the best paras. Unfortunately, due to time constaint and my device taking too long to train the model, I can only perform simple GridSearchCV. So using just the baseline model the accuracy was **0.42**, after doing a simple GridSearchCV, I find this params **{'bootstrap': False, 'max_depth': 150, 'n_estimators': 300}** and using it, I was only able to achieve **0.46** accuracy which is sadly low. The metric I used are Precision, which is the number of correct positive prediction for that category out of the total positive prediction for this category, recall, which is the number of correct positive prediction for that category out of the actual number of that category that should be positive,f1 score which is 2*(recall * precision)/recall + precision and accuracy, the chances of a correct prediction. 

## Other considerations for deploying the models
Unfortunately due to time constraint and my device taking too long for the model to fit the data and GridSearchCV to find the best params, I was only able to produce that model. However, I still believe that there are definitely better parameters that I am yet to find that will allow the random forest classifier to achieve a higher accuracy and f1 score. Furthermore, if there wasnt any time constraint, I would have tried to encode the variables differently like one hot encoding or use a scaler and check if it affect the model performance before deploying as well. 
