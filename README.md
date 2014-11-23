### Objective:

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis.

### Steps:

1. First section is get the raw dataset from the website 

2. Load them to the variable tmp1, tmp2 for the X variable, Y Variable and Subject Variable and merges the training and the test sets to create one data set.

3. Extracts only the measurements on the mean and standard deviation for each measurement.

4. Adding the descriptive activity names to name the activity in the data set

5. labelling the data set with descriptive activity names, and exporting to create a new file "merged_data_set.txt"  ( 10299x68 data frame)

6. Creating tidy data set with the average of each variable for each activity and each subject. and creates a new file "merged_data_set_with_average.txt"
	It is 180x68 because there are 30 subjects and 6 activities, thus "for each activity and each subject" means 30*6=180 rows. 
