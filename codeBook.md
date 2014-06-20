Codebook.md
      - what variables I used
      - names of columns and why
     -  units of data if known
     - reference or link to original data info

I have downloaded the datasets from the given link.
Source-https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

First of all, I read the following files from the test and train folders:
activity_labels.txt : Links the class labels with their activity name                                           [6    x 1]                                   
features.txt        : list of all features                                                                      [561  x 1]
subject_test.txt    : The file contains the code numbers of the subject for each line item in the test dataset  [2947 x 1]
subject_train.txt   : The file contains the code numbers of the subject for each line item in the train dataset [7352 x 1]
X_test.txt          : The file containes values of all features measured for the subjects mentioned in subject_test.txt   [2947 x 561]
X_train.txt         : The file containes values of all features measured for the subjects mentioned in subject_train.txt  [7352 x 561]
Y_test.txt          : Activities performed by the subjects corresponding to the subjects of the test dataset.   [2947 x 1]
Y_train.txt         : Activities performed by the subjects corresponding to the subjects of the tRAIN dataset.  [7352 x 1]

Then I merged the test and train datasets.


  subject_test<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/test/subject_test.txt")
  subject_train<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/train/subject_train.txt")
  subject_total<-rbind(subject_test,subject_train)
  colnames(subject_total)="subject"
  activity_labels<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/activity_labels.txt")
  activity_labels<-activity_labels[,-1]
  features<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/features.txt")
  features<-features[,-1]
  X_test<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/test/X_test.txt")
  X_train<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/train/X_train.txt")
  X_total<-merge(X_test,X_train, all.x=TRUE, all.y=TRUE)
  colnames(X_total)<-features
  
  Y_test<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/test/Y_test.txt")
  Y_train<-read.table("C:/Amar/4 R/Getting and Cleaning Data/UCI HAR Dataset/train/Y_train.txt")
  Y_total=rbind(Y_test,Y_train)
  colnames(Y_total)<-"activity"
  
  t<-grepl("mean\\()|std\\()",colnames(X_total))
  X_total1<-X_total[,t]
  X_final<-cbind(subject_total,Y_total,X_total1)
  
  X_final[X_final$activity==1,2]="WALKING"
  X_final[X_final$activity==2,2]="WALKING_UPSTAIRS"
  X_final[X_final$activity==3,2]="WALKING_DOWNSTAIRS"
  X_final[X_final$activity==4,2]="SITTING"
  X_final[X_final$activity==5,2]="STANDING"
  X_final[X_final$activity==6,2]="LAYING"
  X_final_tidied<-aggregate(x=X_final[,3:68], by = X_final[,1:2], FUN = "mean")

     
