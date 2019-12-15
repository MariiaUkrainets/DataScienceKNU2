Lab5
The raw data for this is the data from University of California Irvine (UCI) "Human Activity Recognition Using Smartphones Data Set". The data contain recordings for 30 volunteers, performing activities of daily living (ADL) while carrying a waist-mounted smartphone with embedded inertial sensors. Here is the link for the reference:
  
  [http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones]

Requirements
Requirements were to create the following R program:
  
the program gets the data, assign labels
merge test and train datasets
use meaningful label for the aggregative variable names
have a tidy data with the mean and SD variables for each subject-activity as the output.
R Code
The code is written in the file 'run_analysis.R'. The assumption is the proper data (from UCI Machine Learning Repository - https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) is saved and placed to the current working directory.

Program consists of 5 blocks:
  
Get the raw data, assign descriptive labels;
Unite test and train data together;
For each testing pull out only mean and standard deviation measurement;
Use activities performed as the activity labels in dataset;
Create independent tidy dataset with means for each variable-activity-subject.

At the end 'tidy_dataset.csv' is saved as the output.