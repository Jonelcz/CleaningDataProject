---
title: "Readme"
author: "Jonel Ccente Zuzunaga"
date: "7/10/2020"
output: html_document
---

## Steps of tidy script 
Copy folder downloaded "UCI HAR Dataset" in working Directory

## Load files for Training
```
my_data_train <- read.table("UCI HAR Dataset/train/X_train.txt")
my_data_train_labels <- read.table("UCI HAR Dataset/train/y_train.txt")
my_subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt")
```

## Load files for Test
```
my_data_test <- read.table("UCI HAR Dataset/test/X_test.txt")
my_data_test_labels <- read.table("UCI HAR Dataset/test/y_test.txt")
my_subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")
```

## Load files with information of variables and subjects
```
my_features <- read.table("UCI HAR Dataset/features.txt")
my_activity_labels <- read.table("UCI HAR Dataset/activity_labels.txt")
```

## Rename variables names according information in features file previuously loaded for train and test data
```
colnames(my_data_train) <- my_features$V2
colnames(my_data_train_labels) <- "activity"
colnames(my_subject_train) <- "subject"
colnames(my_data_test) <- my_features$V2
colnames(my_data_test_labels) <- "activity"
colnames(my_subject_test) <- "subject"
```
## Join columns in a unique dataset for train and other for test, i.e add two new columns "subjects"" and "train labels activities"
```
my_data_train <- cbind(my_subject_train, my_data_train_labels,my_data_train)
my_data_test <- cbind(my_subject_test, my_data_test_labels,my_data_test)
```

## Join all rows from data_train and data_test in a new dataset 
```
my_dataset <- rbind(my_data_test,my_data_train)
```

## Extract only columns for mean and standard deviation for each measurement 
```
my_dataset_select <- my_dataset %>% select(subject,activity,contains(c("mean","std")))
```

## Change de names in activity_labels to join with the previous data
```
colnames(my_activity_labels) <- c("activity","activityname")
```

## Obtain final data set mergering name of activities with dataset
```
my_dataset_tidy <- merge(my_activity_labels, my_dataset_select, by.x = "activity", by.y = "activity")
```

## Finally create a new tidy dataset with the average of each variable for each activity and each subject
```
my_dataset_tidy_summary <- my_dataset_tidy %>% group_by(subject, activity, activityname) %>% summarize_all(list(mean))
```

## Write tidy datasets
```
write.table(my_dataset_tidy, file = "my_dataset_tidy.txt", row.names = FALSE)
write.table(my_dataset_tidy_summary, file = "my_dataset_tidy_summary.txt", row.names = FALSE)
```

