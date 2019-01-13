

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
[img_confusion matrices]: ./pictuers/trained_svm.png
[PR2_view]: ./pictuers/upload.png  


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



