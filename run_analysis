##This script requires you to have the packages "data.table" and "reshape2" installed
##Check if the package are installed and install them otherwise, then load them

if (!require("data.table")) {
  install.packages("data.table")
}
library("data.table")
if (!require("reshape2")) {
  install.packages("reshape2")
}
library("reshape2")

##Load Activity Labels
ActLabels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]

##Load Column Names
features <- read.table("./UCI HAR Dataset/features.txt")[,2]

##Extract ONLY the measurements on the mean and standard deviation for each measurement

ExtractFeat <- grepl("mean|std", features)

##Load and process X_test & y_test data

X_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")

names(X_test) = features

##Extract ONLY the measurements on the mean and standard deviation for each measurement

X_test = X_test[,ExtractFeat]

##Load Activity Labels
y_test[,2] = ActLabels[y_test[,1]]
names(y_test) = c("Activity_ID", "Activity_Label")
names(subject_test) = "subject"

##Bind the data
test_data <- cbind(as.data.table(subject_test), y_test, X_test)

##Load and process X_train & y_train data

X_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")

subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")

names(X_train) = features

##Extract ONLY the measurements on the mean and standard deviation for each measurement

X_train = X_train[,ExtractFeat]

##Load activity data

y_train[,2] = ActLabels[y_train[,1]]
names(y_train) = c("Activity_ID", "Activity_Label")
names(subject_train) = "subject"

##Bind the X and y data

train_data <- cbind(as.data.table(subject_train), y_train, X_train)

##Merge the Test and Training data

data = rbind(test_data, train_data)

id_labels   = c("subject", "Activity_ID", "Activity_Label")
data_labels = setdiff(colnames(data), id_labels)
melt_data      = melt(data, id = id_labels, measure.vars = data_labels)

##Apply mean function to dataset using dcast function

tidy_data= dcast(melt_data, subject + Activity_Label ~ variable, mean)

##Create the tidy data archive

write.table(tidy_data, file = "./tidy_data.txt",row.name=FALSE)
