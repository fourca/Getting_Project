This is the script I finalised to tidy and summarise the Samsung dataset.
I downloaded it on my computer, I assume it won't be a problem but just in case: 
- know where your workind directory is (getwd())
- know how to change it to access your files (setwd(""))
For more details, see the CODEBOOK. 


library(dplyr)
##1) Upload datasets, all data sets are double, one for the train set, the other for the test 
#test_ and train_subjects: each row identifies subjects for each set.
#Range: 1-30
train_subject<-read.table("subject_train.txt")
test_subject<-read.table("subject_test.txt")

# features are the names of the measurements made for each activity e.g. tBodyAcc - mean()-X
features<-read.table("features.txt")
features<-as.vector(features$V2)
# x_test and x_train : measurements results for each activity.
x_test<-read.table("X_test.txt")
x_train<-read.table("X_train.txt")
# activity_labels : 2 column df that links the class labels with their activity name e.g 1 Walking
# Range: 1-6
activity_labels<-read.table("activity_labels.txt")
# y_test and y_train : single columns df with the activity class labels corresponding to each measurement.
y_test<-read.table("y_test.txt")
y_train<-read.table("y_train.txt")

## 2) Fit activity labels and sujects to their belonging sets: 

# Fit activity labels: merge y_ and activity_labels
# and only select the column with descriptive labels (V2 after merging)
# creates a df called (test/train)_labels
train_labels<-merge(y_train, activity_labels, by.x="V1", by.y="V1", all=T)
train_labels<-select(train_labels, V2)
test_labels<-merge(y_test, activity_labels, by.x="V1", by.y="V1", all=T)
test_labels<-select(test_labels, V2)

# Fit subjects: bind (test/train)_subject and (test/train)_labels
# creates a df called (train/test)_labels_subjects
train_labels_subjects<-cbind(train_subject, train_labels)
test_labels_subjects<-cbind(test_subject, test_labels)
# add the appropriate column names
colnames(train_labels_subjects)<-c("Subject","Activity")
colnames(test_labels_subjects)<-c("Subject","Activity")


## 3) Add the measurements names to the measurements results
colnames(x_train)<-features
colnames(x_test)<-features

## 4) Create both train and test sets by merging (train/test)_labels_subjects and x_(train/test)
train<-cbind(train_labels_subjects, x_train)
test<-cbind(test_labels_subjects, x_test)

## 5) Merge both train and test sets
# creates a dataframe called samsung
samsung<-rbind(train, test)

##6) Extract the measurements on the mean and standard deviation for each measurement.

# create a vector with the columns of the aforementioned measurements (we only want mean() and std())
# beware there is a variable called meanFreq which doesn't interest us in this exercise
# that's why on should include the '(' in the research.
samsung_m_std<-grep("mean\\(|std", names(samsung))
# add the 'Subject' and 'Activity' columns (columns 1 and 2 in our samsung dataset)to your selection vector
to_select<-c(1,2,samsung_m_std)
# create the df with the necessary variables: samsung2
samsung2<-samsung[,to_select]
# make the column names slightly more readable
names(samsung2)<-gsub("-","_", names(samsung2))
names(samsung2)<-gsub("\\()","", names(samsung2))
samsung2<-arrange(samsung2, Subject, Activity)
View(samsung2)

##7) Create a separate dataframe with the mean of each variable for each activity of each subject
samsung2_means<-summarise_all(group_by(samsung2, Subject, Activity), mean)
View(samsung2_means)

