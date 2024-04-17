### Load required libraries

library(dplyr)

### Step 1: Download the dataset

### Assuming the dataset is already downloaded and extracted under the folder UCI HAR Dataset

### Step 2: Assign each data to variables

features &lt;- read.table(“UCI HAR Dataset/features.txt”, header =
FALSE) activities &lt;- read.table(“UCI HAR
Dataset/activity\_labels.txt”, header = FALSE) subject\_test &lt;-
read.table(“UCI HAR Dataset/test/subject\_test.txt”, header = FALSE)
x\_test &lt;- read.table(“UCI HAR Dataset/test/X\_test.txt”, header =
FALSE) y\_test &lt;- read.table(“UCI HAR Dataset/test/y\_test.txt”,
header = FALSE) subject\_train &lt;- read.table(“UCI HAR
Dataset/train/subject\_train.txt”, header = FALSE) x\_train &lt;-
read.table(“UCI HAR Dataset/train/X\_train.txt”, header = FALSE)
y\_train &lt;- read.table(“UCI HAR Dataset/train/y\_train.txt”, header =
FALSE)

### Step 3: Merge the training and test sets

X &lt;- rbind(x\_train, x\_test) Y &lt;- rbind(y\_train, y\_test)
Subject &lt;- rbind(subject\_train, subject\_test) Merged\_Data &lt;-
cbind(Subject, Y, X)

### Step 4: Extract only the measurements on the mean and standard deviation

TidyData &lt;- Merged\_Data %&gt;% select(subject, V1, V2:V561) %&gt;%
rename(activities = V1, code = V2) %&gt;% mutate(activities =
activities\[activities$V1 == code, 2\]) %&gt;% select(-code)

### Step 5: Create a second tidy data set with the average of each variable for each activity and each subject

FinalData &lt;- TidyData %&gt;% group\_by(subject, activities) %&gt;%
summarise\_all(mean)

### Export FinalData to FinalData.txt

write.table(FinalData, file = “FinalData.txt”, row.names = FALSE)
