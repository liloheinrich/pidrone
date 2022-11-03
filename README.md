# pidrone
Computer Vision Project, Computational Robotics Fall 2022  

Tigey Jewell-Alibhai and Lilo Heinrich

### Goal
Make a drone fly autonomously towards a color-detected object.

### Solution
- (system diagram here)

This project was equally about systems design and computer vision. On the systems side, we worked on creating a drone platform which can be flown from ROS on the Raspberry Pi. On the Computer Vision side, we worked on getting a raspberry pi camera working and streaming the image data to be able to view it on a laptop from the ground, as well as color-based object detection. We got started by following the examples of using ROS on the ArduCopter website.

### Design Decisions
The color-based detection algorithm used the following steps to process the image:  
1. Hue-Saturation-Value filter (minimum and maximum value thresholds calibrated as needed)    
  a. If the color is within the threshold ranges it is a 1 in the binary mask, otherwise 0.  
2. Find & Filter Contours on minimum area, width, height, solidity, and ratio  
  a. Helps filter out noise or things that aren't actually objects of the right size   
3. Find the x & y pixels as well as width and height of the contour with the largest area. If none, then no object detected.   
  a. Picking the object with the largest area is likely the object that we want it to detect (the best match or closest object of the correct color).  
4. To fly, hold at a certain altitude and yaw with an angular velocity proportional to how many pixels the x-coordinate is from the center of the screen.  

This color-detection algorithm is rather simple but also very effective. To make this algorithm work, we needed an object with a contrasting color to our background so we chose a neon yellow helmet. One weakness is that under different lighting conditions/environments our algorithm might have to be recalibrated, but this was not a problem for us since the neon color was so distinctive and also because recalibrating wasn't too difficult. Using the GRIP program we can visualize the output real-time as we change the pipeline's parameters.

The camera stream was lagging significantly on the offboard laptop, so onboard control was a smart decision, as well as keeping our resolution low to keep the program running faster and take less time to process images. We wanted to put the raspi on the OLIN-DEVICES network rather than its own wifi network to increase the drone's autonomous range, but unfortunately this change messed up our ability to communicate to the drone in ROS because we needed to communicate to the pi through its hostname which was not working on OLIN-DEVICES, so we reverted this change. 

### Challenges
Some challenges we encountered were that system setup took about two weeks of time and we were often using a monitor to troubleshoot or visualize what was going on on the raspberry pi. We also didn't have propellers until the last week, and then during flight testing discovered that controlling the drone to fly needed some troubleshooting as it was not behaving as we had expected/hoped for. Also, we are still unable to echo many mavros topics and think there must be a setup issue. We'd really like to be able to echo mavros topics so that the pi can understand the state of the drone and also so that we can debug the drone control issues more effectively. It turns out autonomous flight is a difficult thing to implement and integration takes a long time. In retrospect, it might have been better to start actually flying the droone and working on the control and localization side earlier in the project.

### Improvements
Going forward, we've built up this drone platform and we'd like to continue working with it in our final project. Some things we would like to work on are developing more control over the drone's movements, detecting and localizing the drone based on apriltags, or perhaps even adding a realsense camera.

### Lessons Learned
Start flight testing earlier.
