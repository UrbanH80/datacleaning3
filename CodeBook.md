This code book describes the variables, the data, and transformations performed to clean up the data for datacleaning3



**Variables**

Subject:  Unique identifier of someone who had their fitness data recorded

Activity: "walking"   "walking_upstairs" "walking_downstairs" "sitting" "standing" "laying"  

Feature Labels:  561 Features were recorded.  More detailed information on the features can be found from the authors
of the data in the features.txt and features_info.txt files uploaded to this github.




**Data**

The data investigated here was collected from  accelerometers from the Samsung Galaxy S smartphone. The site where the data was obtained is:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

The data for the project is here:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Data acknowledgement:
Use of this dataset in publications must be acknowledged by referencing the following publication [1] 
[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012
This dataset is distributed AS-IS and no responsibility implied or explicit can be addressed to the authors or their institutions for its use or misuse. Any commercial use is prohibited.
Jorge L. Reyes-Ortiz, Alessandro Ghio, Luca Oneto, Davide Anguita. November 2012.


**Transformations**

The run_analysis.R script in this repo includes the following work.  Note:  A more detailed description of the transformations 
is also available in the R script attached in the github repo.


- Merges the training and the test sets to create one data set.  Two train and test data sets were created by adding the 
activity and subject ID to the data sets with variable titles (from the feature labels).  The test and train data sets were then 
combined using the rbind function.


- Extracted the mean and standard deviation for each measurement by using the gprep function to look for mean() and std(). 


- Replaced the activity identifier (numbers 1 through 6) with the full description of the activity 


- Lightly cleaned up the varible names by removing "()" and replacing "-" with underscore using the gsub function.


- Used the dplyr group_by and summarise functions to take the average of each variable for each activity and each subject.




