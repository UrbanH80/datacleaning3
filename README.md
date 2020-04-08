# datacleaning3
This repo explains how the scripts used for the project work and how they are connected.

#Download the file
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "gdp.csv", method = "curl")  #unzipped manually



############ 1 Merge training and the test sets to create one data set

##### Training set
featurelabels <- read.table("./UCI HAR Dataset/features.txt", stringsAsFactors = FALSE)  #pull in name of features, should be 561
featurelabels <- as.vector(featurelabels[[2]])    #vector of relevant column

traindata <- read.table("./UCI HAR Dataset/train/X_train.txt")    #pulls in actual training data
colnames(traindata) <- featurelabels   #apply feature names to each 561 variables

subject <- read.table("./UCI HAR Dataset/train/subject_train.txt")   #pulls in who subject is for each  7352 obs
subject <- as.vector(subject[[1]])             #vector of relevant column


activity <- read.table("./UCI HAR Dataset/train/y_train.txt")  #pulls in activity for each 7352 obs
activity <- factor(activity[[1]])               #vector of relevant column


traindata <- cbind(subject, activity, traindata)     #combine to one data set

####### Test set
testdata <- read.table("./UCI HAR Dataset/test/X_test.txt")
colnames(testdata) <- featurelabels   #apply feature names to each 561 variables

subject <- read.table("./UCI HAR Dataset/test/subject_test.txt")
subject <- as.vector(subject[[1]])

activity <- read.table("./UCI HAR Dataset/test/y_test.txt")
activity <- factor(activity[[1]])

testdata <- cbind(subject, activity, testdata)     #combine to one data set



#######Merge Train and Test
traintest <- rbind(traindata, testdata)


############### 2 Extract only the measurements on the mean and standard deviation for each measurement
traintest1 <- traintest[, c(1, 2, grep("mean\\(\\)|std\\(\\)", names(traintest)))]
        #want all rows
        #grep only features.  Had to add c(1,2...) otherwise traintest1 wasn't returning subject or activity
        #using \\(\\) so don't get only mean or std, for example don't want "meanFreq()"
dim(traintest1)






############### 3 Use descriptive activity names to name the activities in the data set
activitylabels <- read.table("./UCI HAR Dataset/activity_labels.txt")   #pull in activity names

activitylabels <- tolower(as.vector(activitylabels[[2]]))   #make vector of lowercase characters

levels(traintest1$activity) <- activitylabels     #swap out activity labels for activity column in traintest1





############### 4 Appropriately labels the data set with descriptive variable names. 
#didn't really want to change all the labels, they seem pretty explanitory so going to remove some of the characters
labels <- gsub("()", "", names(traintest1), fixed = TRUE)   #removing (), storing labels to "labels"
labels <- gsub("-", "_", labels)                            #replacing "-" with underscore in new labels object
names(traintest1) <- labels                                 #load labels into traintest1 dataset so it is saved




############## 5 From the data set in step 4, creates a second, independent tidy data set 
##############   with the average of each variable for each activity and each subject.

library(dplyr)     #loading dplyr to use the group by function to get average for each activity and subject


meandata <- group_by(traintest1, activity, subject)  %>%      #use group_by to group activity and subject and summarize all for mean()
        summarise_all(funs(mean))                             #using summarise_all() to apply to every variable


write.table(meandata, "tidymeandata.txt", row.names = FALSE)    #writing out final tidy data table so it can be uploaded to github
