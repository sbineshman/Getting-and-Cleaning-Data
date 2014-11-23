### Introduction

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

<!-- -->

###first section of the download the file from the website 

fileUrl<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "Dataset.zip" , mode="wb")


####  1. Load them to the variable tmp1, tmp2 for the X variable, Y Variable and Subject Variable and merges the training and the test sets to create one data set.

tmp1 <- read.table("train/X_train.txt")
tmp2 <- read.table("test/X_test.txt")
X <- rbind(tmp1, tmp2)

tmp1 <- read.table("train/subject_train.txt")
tmp2 <- read.table("test/subject_test.txt")
S <- rbind(tmp1, tmp2)

tmp1 <- read.table("train/y_train.txt")
tmp2 <- read.table("test/y_test.txt")
Y <- rbind(tmp1, tmp2)

#### 2. Extracts only the measurements on the mean and standard deviation for each measurement.

features <- read.table("features.txt")
indices_of_good_features <- grep("-mean\\(\\)|-std\\(\\)", features[, 2])
X <- X[, indices_of_good_features]
names(X) <- features[indices_of_good_features, 2]
names(X) <- gsub("\\(|\\)", "", names(X))
names(X) <- tolower(names(X))  

##### 3. Adding the descriptive activity names to name the activity in the data set

activity <- read.table("activity_labels.txt")
activity[, 2] = gsub("_", "", tolower(as.character(activity[, 2])))
Y[,1] = activity[Y[,1], 2]
names(Y) <- "activity"

#####  4. labeling the data set with descriptive activity names, and exporting to create a new "file merged_data_set.txt"

names(S) <- "subject"
cleaned <- cbind(S, Y, X)
write.table(cleaned, "merged_data_set.txt", row.names = FALSE)

#####  5. Creates tidy data set with the average of each variable for each activity and each subject. and creates a new file "merged_data_set_with_average.txt"

uniqueSubjects = unique(S)[,1]
numSubjects = length(unique(S)[,1])
numactivity = length(activity[,1])
numCols = dim(cleaned)[2]
result = cleaned[1:(numSubjects*numactivity), ]

row = 1
for (s in 1:numSubjects) {
        for (a in 1:numactivity) {
                result[row, 1] = uniqueSubjects[s]
                result[row, 2] = activity[a, 2]
                tmp <- cleaned[cleaned$subject==s & cleaned$activity==activity[a, 2], ]
                result[row, 3:numCols] <- colMeans(tmp[, 3:numCols])
                row = row+1
        }
}
write.table(result, "merged_data_set_with_average.txt", row.names = FALSE)