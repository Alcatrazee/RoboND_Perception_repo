

## Project:Perception  

Steps to complete the project:

  1.Make sure you have already setup your ROS Workspace in the VM provided by Udacity or on your own local Linux/ROS install. If you're not already setup, you can find instructions here.  
  2.Complete Perception Exercises 1, 2 and 3, which comprise the project perception pipeline.  
  3.Download or clone the project repository.  
  4.Follow the steps laid out in this [lesson](https://classroom.udacity.com/nanodegrees/nd209/parts/c199593e-1e9a-4830-8e29-2c86f70f489e/modules/e5bfcfbd-3f7d-43fe-8248-0c65d910345a/lessons/e3e5fd8e-2f76-4169-a5bc-5a128d380155/concepts/802deabb-7dbb-46be-bf21-6cb0a39a1961).  

[img_dowmsample]: ./pictuers/EX1_downsampled.png
[img_passthrough]: ./pictuers/EX1_pass_through_filter.png
[img_extracted_inliers]: ./pictuers/EX1_extracted_inliers.png
[img_extracted_outliers]: ./pictuers/EX1_extracted_outliers.png
[img_clusters]: ./pictuers/EX2_clusters.png
[img_confusion matrices]: ./pictuers/EX3_trained_svm.png
[PR2_view]: ./pictuers/upload.png  
[world1]: ./pictuers/world1.png  
[world2]: ./pictuers/world2.png 
[world3]: ./pictuers/world3.png 
[newest_img]: ./pictuers/all_fine.png

This is a project to implement proception stage of a robot.  
### Excercise 1  
#### Requirement:  
Implement basic operation of point cloud with python point cloud library.  
##### Voxel downsampling  
Voxel is a unit of point cloud,just like pixel to a image.Each voxel contain information of position and color(in this project).The original point cloud is of high density,in order to make the process faster,downsample the point cloud to reduce the computational complexity.It's like reducing resolution of images.Less voxel,faster process in future.  

![alt text][img_dowmsample]  
This is a screenshot of downsampled point cloud  
By using point cloud library,it's easy to implement voxel downsampling.  
##### Passthrough filter
Passthrough filter is easy to understand,just like clipping a picture,passthrough filter 'cut' a point cloud and throw away the useless region.  
![alt text][img_passthrough]  
This is the screenshot of point cloud after passthrough filter.In this case,'cut' everything else except for the table and things on the table.   
##### segmentation  
In this case,use RANSAC to segment table and stuffs on the table.RANSAC(Random sample consensus) is an iterative method to estimate parameters of a mathematical model from a set of observed data that contains outliers.Unlike least square fit method,RANSAC will cut some of the data loose to have a better result.In this case,we just need to segment the table with a plane equation,then the rest is the outliers(we can get to it by set the parameter 'negative' to true).  
![alt text][img_extracted_inliers]  
This is the screenshot of inliers(table).
![alt text][img_extracted_outliers]  
This is the screenshot of outliers(Things on the table).
### Excersice 2  
#### Requirement:  
Implement ecludiean cluster extraction to point cloud after segmentation.  
By applying k-d tree method,we can cluster the segmented point cloud into pieces.The algorithm only require position of voxel,therefore,we get rid of the color information by using function from **pcl_helper.py**.Point cloud library offered convenient tools to do the job,we just need to tune the parameters.In this case,the acceptable parameters are(tolerance=0.001,minclustersize=0,maxclustersize=2000).
###### k-d tree  
k-d tree (short for k-dimensional tree) is a space-partitioning data structure for organizing points in a k-dimensional space.  
In a bunch of data of k dimension(x1,x2,x3,...,xk),here's the way to build the k-d tree:  
    1.Get median of data of the n<sup>th</sup> dimension,n = 1,2,3...  
    2.Traverse every data to seperate data into two branches,whose left is the data less than the median,and right side is the data bigger than median.  
    3.Traverse each branches,apply step 1 with n+1<sup>th</sup> dimension till n=k.
After these process,we have a k-d tree.(It's really hard to express in English...)
In this scenario,we can seperate voxels into each grid.Then we can extract these point cloud.  

![alt text][img_clusters]
This is the screenshot after eclidean cluster extraction.  

### Excersice 3 
#### Requirement:  
Train a SVM and use it to recognize things on the table.
###### SVM  
In machine learning, support-vector machines (SVMs, also support-vector networks) are supervised learning models with associated learning algorithms that analyze data used for classification and regression analysis.In this case,I used linear method to construct the classifier.
###### Trainning
The important part of a trainning is feature extraction.In this project,we used histogram of HSV(or RGB) and normal as feature to train the SVM.By concatenate these two histogram,we obbtain feature list which is the trainning set we are going t o use.  
###### Result  
After trainning,the svm evaluation result seems pretty good,since I changed all options which the lesson pointed out that can increase the accuracy.Here's the  confusion matrices.  
![alt text][img_confusion matrices]  

### project:Pick and Place
Pick and Place Setup
1. For all three tabletop setups (test*.world), perform object recognition, then read in respective pick list (pick_list_x.yaml). Next construct the messages that would comprise a valid PickPlace request output them to .yaml format.
These are the pictures of the PR2 perception and recognize things on the table. (Accuracy is still a problem!)
![alt_text][world1]  ![alt_text][world2]  ![alt_text][world3]   
In world 1,accuracy is 100%,in world 2,there's one mistake,and also one mistake on world 3.  
Outputx.yaml are listed in the root folder.These files are not output in the same running as these pictures,so there will be some difference between them. 
In the program,first thing it does is to rotate the PR2 to look aroud to contruct a collision map,after that,we start to cluster and segement and recognize stuffs on the table.After recognizing all things,we are ready to make a dictionary and output yaml as well as call the service which drive the PR2 to pick.  
However,there's some error occur in the process of coding.Such as data type error which almost drove me crazy, and I even rewrote the code of udacity to make my code run successfully,but in the end,I managed to fix the error and changed it back.Anyway,python is a very easy-to-use programming language,however the efficiency and data type concerns me.I'm looking forward to program with c++.

## Resummit changes.  
1.Alterned the voxel grid parameter into half of before,then tunned the parameters of cluster and segmentation function.Now it can recognize all the things in world 3.Yeah~
![alt text][newest_img]
