# Package
library(data.table)

# Set path for fread
path <- file.path(getwd(), "UCI HAR Dataset")

# Read files
SubjectTrain <- fread(file.path(path, "train", "subject_train.txt"))

SubjectTest <- fread(file.path(path, "test", "subject_test.txt"))

TrainingLabels <- fread(file.path(path, "train", "y_train.txt"))

TestLabels <- fread(file.path(path, "test", "y_test.txt"))

TrainingSet <- fread(file.path(path, "train", "X_train.txt"))

TestSet <- fread(file.path(path, "test", "X_test.txt")) 

# Merge test and training subjects
Subject <- rbind(SubjectTrain, SubjectTest)

setnames(Subject, "V1", "subject")

# Merge test and training labels
Labels <- rbind(TrainingLabels, TestLabels)

setnames(Labels, "V1", "activity")

# Merge test and training sets
Set <- rbind(TrainingSet, TestSet)

# Merge subjects, activity labels, and sets
SubjectLabels <- cbind(Subject, Labels)

Set <- cbind(SubjectLabels, Set)

# Features names
features <- fread(file.path(path, "features.txt"))

setnames(features, c("V1", "V2"), c("index", "feature"))

# Extract mean and SD
features <- features[grepl("mean\\(\\)|std\\(\\)", features$feature)]

features$index <- paste0("V", features$index)

Set <- Set[, c("subject","activity",features$index), with = FALSE]

# Descriptive activity labels
activity.labels <- fread(file.path(path, "activity_labels.txt"))

setnames(activity.labels, names(activity.labels), c("index", "name"))

Set <- Set[, activity:=activity.labels$name[Set$activity]]

Set$activity <- factor(Set$activity)

Set$subject <- factor(Set$subject)

# Names of measurement variables
setnames(Set, c("subject", "activity", features$feature[1:66]))

setnames(Set, names(Set), gsub("^t", "Time", names(Set)))

setnames(Set, names(Set), gsub("^f", "Freq", names(Set)))

setnames(Set, names(Set), gsub("-mean\\(\\)", "Mean", names(Set)))

setnames(Set, names(Set), gsub("-std\\(\\)", "SD", names(Set)))

setnames(Set, names(Set), gsub("-", "", names(Set)))

# Tidy dateset
Tidy <- Set[, lapply(.SD, mean), by=c("subject","activity")]

# Write to file
write.table(Tidy, "tidydata.txt", row.names = FALSE)
