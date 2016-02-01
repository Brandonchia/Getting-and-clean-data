<<<<<<< Variables Explainations:

subject: Identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.

activity: Activity name

Time: Time domain

Freq: Frequency

Body: Body acceleration signal

Gravity: Gravity acceleration signal

Acc: Measurement tool is accelerometer

Gyro: Measurement tool is gyroscope

Jerk: Jerk signal

Mag: Signal's magnitude

Mean: Average

SD: Standard deviation

X: X-axis measurement

Y: Y-axis measurement

Z: Z-axis measurement

<<<<<<< Steps:

Step1: Read data with fread function

Step2: Merge test and training subjects, labels, sets with rbind and cbind functions 

Step3: Merge subject, activity labels, and sets with cbind function

Step4: Name the col names of features and index for the sake of clarification 

Step5: Extract  mean and SD

Step6: Set descriptive activity labels

Step7: Names measurement variables

Step8: Create tidy data subset with average of each variable for each activity and each subjects.

Step9: Write .txt file with write.table function

