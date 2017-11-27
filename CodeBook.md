GETTING AND CLEANING DATA - WEEK 4 - COURSE PROJECT: 

THIS CODEBOOK CONTAINS:
- Name of the study used for this assignment
- Authors of the study 
- Methods of the study
- Used datasets from the original downloadable data (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
- Necessary R packages for the assignment
- Names of the created dataframes 
- Explanation on the way the data was tidied and merged.
- Relevant dataframes containing the demanded results (names)
- Column names with explanation on how the measurement columns were coded.


==================================================================
NAME : Human Activity Recognition Using Smartphones Dataset Version 1.0

AUTHORS: Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory
DITEN - Università degli Studi di Genova.
Via Opera Pia 11A, I-16145, Genoa, Italy.
activityrecognition@smartlab.ws
www.smartlab.ws

METHODS: The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 
For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.


USED DATASETS FROM THE ORIGINAL .ZIP:
- 'features.txt': List of all measurements.
	=> loaded as 'features'
- 'activity_labels.txt': Links the class labels with their activity name.
	=> loaded as 'activity_labels'
- 'train/X_train.txt': Training set (results).
	=> loaded as 'x_train'
- 'train/y_train.txt': Training activity labels.
	=> loaded as 'y_train'
- 'test/X_test.txt': Test set (results)
	=> loaded as 'x_test'
- 'test/y_test.txt': Test activity labels.
	=> loaded as 'y_test'
- 'train/subject_train.txt', 'test/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30. 
	=> loaded as 'test_subject' and 'train_subject'


NECESSARY PACKAGES: 
dplyr


CREATED DATAFRAMES: 
- 'train_labels' 
	= y_train + activity labels
- 'test_labels'
	= y_test + activity labels
- 'train_labels_subjects' 
	= train_labels + train_subject
- 'test_labels_subjects' 
	= test_labels + test_subject
- 'train' 
	= 'train_labels_subjects' + 'x_train'
- 'test' 
	= 'test_labels_subjects' + 'x_test'
- 'samsung' 
	= 'test' + 'train'
- 'samsung2' 
	= only mean and standard deviation measurements of 'samsung'
- 'samsung2_means' 
	= means of each measurement for each subject's activity.

TIDYING AND MERGING THE DATA:(also included in the script)
1) Upload datasets, all data sets are double, one for the train set, the other for the test (see above 'USED DATASETS FROM THE ORIGINAL ZIP)
2) Fit activity labels and sujects to their belonging sets: 
- Fit activity labels: merge y_ and activity_labels
- and only select the column with descriptive labels (V2 after merging)
=> creates a df called (test/train)_labels
3) Fit subjects: 
- rbind (test/train)_subject and (test/train)_labels
- add the appropriate column names ("Subject","Activity")
=> creates a df called (train/test)_labels_subjects
4) Add the column names to the measurements using colnames on x_(test/train) with features
5) Create both train and test sets: 
- rbind (train/test)_labels_subjects and x_(train/test)
=> creates 2 df: train and test.
5) Merge both train and test sets
- cbind on test and train
=> creates a dataframe called samsung
6) Extract the measurements on the mean and standard deviation for each measurement.
- create a vector with the columns of the aforementioned measurements (we only want mean() and std()) using grep
- beware there is a variable called meanFreq which doesn't interest us in this exercise
- that's why on should include the '\\(' in the grep research.
- add the 'Subject' and 'Activity' columns (columns 1 and 2 in our samsung dataset)to your selection vector
- subset the df with the necessary variables
=> creates samsung2
7) Make the column names slightly more readable by replacing "-" with "_" and removing the "()" using gsub.
8) Create a separate dataframe with the mean of each variable for each activity of each subject using dplyr's summarize.


NAME OF THE RELEVANT DATAFRAMES WITH RESULTS: 
- 'samsung2'
- 'samsung2_means'


MEASUREMENTS (COLUMN) NAMES:
- Subject: subject's ID (ranges 1 to 30)
- Activity: 6 levels: 
	- WALKING
	- WALKING_UPSTAIRS
	- WALKING_DOWNSTAIRS 
	- SITTING
	- STANDING
	- LAYING 
- Signal estimations: 
"tBodyAcc_mean_X"           "tBodyAcc_mean_Y"           "tBodyAcc_mean_Z"           "tBodyAcc_std_X"            
"tBodyAcc_std_Y"            "tBodyAcc_std_Z"            "tGravityAcc_mean_X"        "tGravityAcc_mean_Y"        
"tGravityAcc_mean_Z"        "tGravityAcc_std_X"         "tGravityAcc_std_Y"         "tGravityAcc_std_Z"         
"tBodyAccJerk_mean_X"       "tBodyAccJerk_mean_Y"       "tBodyAccJerk_mean_Z"       "tBodyAccJerk_std_X"        
"tBodyAccJerk_std_Y"        "tBodyAccJerk_std_Z"        "tBodyGyro_mean_X"          "tBodyGyro_mean_Y"          
"tBodyGyro_mean_Z"          "tBodyGyro_std_X"           "tBodyGyro_std_Y"           "tBodyGyro_std_Z"           
"tBodyGyroJerk_mean_X"      "tBodyGyroJerk_mean_Y"      "tBodyGyroJerk_mean_Z"      "tBodyGyroJerk_std_X"       
"tBodyGyroJerk_std_Y"       "tBodyGyroJerk_std_Z"       "tBodyAccMag_mean"          "tBodyAccMag_std"           
"tGravityAccMag_mean"       "tGravityAccMag_std"        "tBodyAccJerkMag_mean"      "tBodyAccJerkMag_std"       
"tBodyGyroMag_mean"         "tBodyGyroMag_std"          "tBodyGyroJerkMag_mean"     "tBodyGyroJerkMag_std"      
"fBodyAcc_mean_X"           "fBodyAcc_mean_Y"           "fBodyAcc_mean_Z"           "fBodyAcc_std_X"            
"fBodyAcc_std_Y"            "fBodyAcc_std_Z"            "fBodyAccJerk_mean_X"       "fBodyAccJerk_mean_Y"       
"fBodyAccJerk_mean_Z"       "fBodyAccJerk_std_X"        "fBodyAccJerk_std_Y"        "fBodyAccJerk_std_Z"        
"fBodyGyro_mean_X"          "fBodyGyro_mean_Y"          "fBodyGyro_mean_Z"          "fBodyGyro_std_X"           
"fBodyGyro_std_Y"           "fBodyGyro_std_Z"           "fBodyAccMag_mean"          "fBodyAccMag_std"           
"fBodyBodyAccJerkMag_mean"  "fBodyBodyAccJerkMag_std"   "fBodyBodyGyroMag_mean"     "fBodyBodyGyroMag_std"      
"fBodyBodyGyroJerkMag_mean" "fBodyBodyGyroJerkMag_std"


The measurements variables were coded as: 
I)'t'= time domain signals:  captured at a constant rate of 50Hz, then filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise.
I)'f'= frequency domain signals: after Fast Fourier Transform (FFT).
II)'Body'= body acceleration signals separated using using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 
II)'Gravity' = gravity acceleration signals separated using using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 
III)'Acc' = accelerometer 
III)'Gyro' = gyroscope 
IV)'Jerk' = jerk signals: derived from body linear acceleration and angular velocity.
IV)'Mag' = magnitude of the three-dimensional signals: calculated using the Euclidean norm
V)'mean' = mean.
V)'std' = standard deviation.
VI)'X' or 'Y' or 'Z' = 3-axial signals in the X, Y and Z directions.



