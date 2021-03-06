# Getting and Cleaning Data Programming Assignment

This project is a programming assignment to the online course
[Getting and Cleaning Data](https://www.coursera.org/learn/data-cleaning).

* [Produced data / Codebook](#pdata)
* [R Script](#rscript)

## <a name="pdata"></a>Produced data / Codebook

When the R script [run_analysis.R](run_analysis.R) is executed, it creates
a file [X_summarized.txt](X_summarized.txt) with average values for each
activity and subject. See the [Codebook](CodeBook.md) for detailed description.

After downloading the file, you may use the following command to load the file in R:

```R
read.table("X_summarized.txt", header = TRUE)
```

## <a name="rscript"></a> R script

This section explains the R script [run_analysis.R](run_analysis.R) used to process the data.

Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

The script assumes that the data has been downloaded and unzipped. The script
should be located in the directory with the unzipped data, that is in the
directory where the file README.txt is located.

Load required libraries for manipulating tables:
```R
library(dplyr)
```

Load source data files. This script assumes we are located in the directory
where the data files were extracted.

```R
features <- read.table("features.txt")
activity_labels <- read.table("activity_labels.txt")

subject_test <- read.table("test/subject_test.txt")
X_test <- read.table("test/X_test.txt")
y_test <- read.table("test/y_test.txt")

subject_train <- read.table("train/subject_train.txt")
X_train <- read.table("train/X_train.txt")
y_train <- read.table("train/y_train.txt")
```

Merge rows of test and train data sets. After merging, clean up memory
by removing original datasets (only memory is cleaned up, files are not
affected).
```R
subject_merged = rbind(subject_test, subject_train)
X_merged = rbind(X_test, X_train)
y_merged = rbind(y_test, y_train)
rm(subject_test, subject_train, X_test, X_train, y_test, y_train)
```

Add descriptive column names to tables related to activities.

The table "y_merged" contains just an activity ID for each row, where rows
correspond the data in the table "X_merged".

```R
colnames(y_merged) <- c("activityId")
```

The table "activity_labels" contains an ID and a human-readable label for
each activity.

```R
colnames(activity_labels) <- c("activityId", "activity")
```

Add descriptive column name to the table with subject IDs. The table contains
just one column (subject ID).

```R
colnames(subject_merged) <- c("subjectId")
```

Assign descriptive column names to the main data table using the available
"features" table.
Remove parentheses from column names to make them better readable.
Since parentheses are meta-characters for sub, we have to quote them.

```R
colnames(X_merged) <- gsub("\\(\\)", "", features$V2)
```

Move "mean" and "std" to the end of variable names to make variable names
more uniform. For example, transform "tBodyAcc-mean-X" into "tBodyAcc-X-mean".

```R
colnames(X_merged) <- gsub("(mean|std)-([X-Z])", "\\2-\\1", colnames(X_merged))
```

Find columns which contains either mean or standard deviation, that is
columns with names ending in "-mean" or "-std". Keep only these columns
and discard the rest.

```R
select_cols = grepl("(-mean|-std)$", colnames(X_merged))
X_merged = X_merged[,select_cols]
```

Replaces "-" characters in column names with "." characters to convenience
of usage. If a column name contains the character "-", the column name can't
be directly used in some R expressions and must be quoted, for example
X_merged$'tBodyAcc-mean-X'. If the character "." is used instead, column names
may be used without quotes.

```R
colnames(X_merged) <- gsub("-", ".", colnames(X_merged))
```

Combined prepared data by columns:

 * Subject ID
 * Mean and standard deviation for each measurement
 * Activity ID.
 
```R
X_merged <- cbind(subject_merged, X_merged, y_merged)
```

Merge main data table with activity labels. Note that we can't merge with
activity labels earlier, since merging changes an order of rows. If we would
merge activity IDs with activity labels before combining activity IDs with
the main data table, the order of activity IDs will change.
We do not need to explicitly specify the column to merge by, because we
already ensured that both tables share the column name "activityId".

```R
X_merged <- merge(X_merged, activity_labels)
```

Reorder columns: exclude activity ID, move activity label to the front.

```R
X_merged <- select(X_merged, subjectId, activity, tBodyAcc.X.mean:fBodyBodyGyroJerkMag.std)
```

Calculate average for each activity and each subject.

```R
X_summarized <- X_merged %>%
    group_by(subjectId, activity) %>%
    summarize_all(funs(mean))
```

Export the summarized data set into a file.

```R
write.table(X_summarized, file="X_summarized.txt", row.names = FALSE)
```
