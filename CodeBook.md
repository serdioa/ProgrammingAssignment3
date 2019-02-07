# Getting and Cleaning Data Programming Assignment: Codebook

This document describes content of the data file created in a programming
assignment to the online course
[Getting and Cleaning Data](https://www.coursera.org/learn/data-cleaning).

* [Source dataset](#sourcedataset)
* [Applied transformations](#transform)
* [Data columns](#datacolumns)
* [References](#references)

## <a name="sourcedataset"></a>Source dataset

This programming assignment is based on the dataset [[1]](#ref1). The dataset
is available online:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

The original dataset contains data collected in experiments carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, the authors of the original dataset captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

Multiple variables were estimated from each signal by applying a set of functions, in particular mean and standard deviation.

## <a name="transform"></a>Applied transformations

This programming assignment starts with the data available in the dataset described in the previous section. Our purpose is to estimate an average mean and standard deviation of each signal for each test subject and activity.

The training and test data sets were merged into a single data set. Meaningful and R-friendly columns names were assigned to the merged data set. By "R-friendly" we mean column names which may be used in R without quoting, that is column names without special characters. All data columns except mean values and standard deviations were removed. The data set was enriched with IDs of test subjects (persons) and labels of activities. 

From that intermediate data set, the target data set was produced. The intermediate data set was grouped by the subject ID and activity label, and averages for all remaining variables were calculated. The produced data set has been exported into the file [X_summarized.txt](X_summarized.txt).

After downloading the file, you may use the following command to load the file in R:

```R
read.table("X_summarized.txt", header = TRUE)
```

The produced file contains tidy data as defined in [[2]](#ref2). Each row contains
one observation. The first two columns contains an ID
of the test subject (person) and an activity. All subsequent columns contains
variables averaged over measurements available in the source data.
For variables representing 3-dimensional vectors, values in X, Y and Z
dimensions are provided. For each variable the mean value (mean) and an average
standard deviation (std) were calculated.

## <a name="datacolumns"></a>Data Columns

**subjectId**

Identifier of the test subject (person).

**activity**

The activity label. The labels are self-describing: WALKING, WALKING_UPSTAIRS,
WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING.

**tBodyAcc.XYZ.mean**, **tBodyAcc.XYZ.std**

3-axial body acceleration normalized between [-1, 1].
Mean and standard deviation are provided.
The body acceleration represents a high-frequency part of the total acceleration
with a corner frequency of 0.3 Hz. 

**tGravityAcc.XYZ.mean**, **tGravityAcc.XYZ.std**

3-axial gravity acceleration normalized between [-1, 1].
Mean and standard deviation are provided.
The gravity acceleration represents a low-frequency part of the total
acceleration with a corner frequency 0.3 Hz.

**tBodyAccJerk.XYZ.mean**, **tBodyAccJerk.XYZ.std**

3-axial time derivative of the body acceleration normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyGyro.XYZ.mean**, **tBodyGyro.XYZ.std**

3-axial gyroscope signal normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyGyroJerk.XYZ.mean**, **tBodyGyroJerk.XYZ.std**

3-axial time derivative of the gyroscope signal normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyAccMag.mean**, **tBodyAccMag.std**

Magnitude of the body acceleration normalized between [-1, 1].
Mean and standard deviation are provided.

**tGravityAccMag.mean**, **tBodyAccMag.std**

Magnitude of the gravity acceleration normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyAccJerkMag.mean**, **tBodyAccJerkMag.std**

Magnitude of the time derivative of the body acceleration normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyGyroMag.mean**, **tBodyGyroMag.std**

Magnitude of the gyroscope signal normalized between [-1, 1].
Mean and standard deviation are provided.

**tBodyGyroJerkMag.mean**, **tBodyGyroJerkMag.std**

Magnitude of the time derivative of the gyroscope signal normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyAcc.XYZ.mean**, **fBodyAcc.XYZ.std**

3-axial body acceleration in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyAccJerk.XYZ.mean**, **fBodyAccJerk.XYZ.std**

3-axial time derivative of the body acceleration in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyGyro.XYZ.mean**, **fBodyGyro.XYZ.std**

3-axial gyroscope signal in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyAccMag.mean**, **fBodyAccMag.std**

Magnitude of the body acceleration in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyAccJerkMag.mean**, **fBodyAccJerkMag.std**

Magnitude of the time derivative of the body acceleration in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyGyroMag.mean**, **fBodyGyroMag.std**

Magnitude of the gyroscope signal in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.

**fBodyGyroJerkMag.mean**, **fBodyGyroJerkMag.std**

Magnitude of the time derivative of the gyroscope signal in frequency domain, normalized between [-1, 1].
Mean and standard deviation are provided.


## <a name="references"></a>References

<a name="ref1"></a>[[1]](https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip) Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

<a name="ref2"></a>[[2]](https://www.jstatsoft.org/article/view/v059i10/v59i10.pdf) Hadley Wickham. Tidy data. The Journal of Statistical Software, vol. 59, 2014.

