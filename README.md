
<img align="right" src="https://github.com/liloheinrich/pidrone/blob/main/IMG_3286_2.gif" width="500"/>

## Pi Drone

Computer Vision Project  
Computational Robotics Fall 2022  
Tigey Jewell-Alibhai and Lilo Heinrich

### Goal
Make a drone fly autonomously towards a color-detected object.

<!-- <img src="https://github.com/liloheinrich/pidrone/blob/main/IMG_3286_2.gif" width="370"/> <img src="https://github.com/liloheinrich/pidrone/blob/main/IMG_3296.gif" width="450"/> -->

<!-- ![PF2](https://github.com/liloheinrich/pidrone/blob/main/IMG_3286_2.gif)
![PF2](https://github.com/liloheinrich/pidrone/blob/main/IMG_3296.gif) -->


### Solution
This project was equally about systems design and computer vision. On the systems side, we created a drone platform which can be flown from ROS on the Raspberry Pi. On the Computer Vision side, we got a raspberry pi camera working and streaming the image data which can be viewed on a laptop from the ground as well as color-based object detection. We got started by following the examples of using ROS on the ArduCopter website.

<img src="https://github.com/liloheinrich/pidrone/blob/main/system_diag_pidrone.PNG" width="800"/>

### Design Decisions
The color-based detection algorithm used the following steps to process the image:  
1. Hue-Saturation-Value filter (minimum and maximum value thresholds calibrated as needed)    
  a. If the color is within the threshold ranges it is a 1 in the binary mask, otherwise 0.  
2. Find & Filter Contours on minimum area, width, height, solidity, and ratio  
  a. Helps filter out noise or things that aren't actually objects of the right size   
3. Find x, y, width, height of the contour with the largest area. If none, then no object detected.   
  a. Picking the object with the largest area is likely the object that we want it to detect (closest/largest object of the correct color).  
4. To fly, hold at some altitude and yaw with an angular velocity proportional to how many pixels the x location is from center.  

<!-- - (picture of grip pipeline, maybe a side by side of seeing t shirt vs not bc backlit) -->
<img align="left" src="https://github.com/liloheinrich/pidrone/blob/main/IMG_3296.gif" width="550"/>

The color-detection algorithm is simple but effective. To make this algorithm work, we needed an object with a contrasting color to our background so we at first chose a neon yellow helmet. However, green grass as a close color to yellow and the helmet was glossy, worsening glare. We switched to tracking purple T-shirts because they had a contrasting and easy to pick out color as well as nonreflective. The weaknesses of color filtering are that under different lighting conditions/environments our algorithm has to be recalibrated, and that the angle of the camera in relation to the sun causes some images to be very backlit which we only realized once we moved outside. Our color tracking algorithm works most of the time but not under very backlit conditions.

The camera stream lagged significantly on the laptop, so onboard control was a smart decision. As was keeping our resolution low to keep the program running faster and take less time to process images. We wanted to put the raspi on the OLIN-DEVICES network rather than its own wifi network to increase the drone's autonomous range, but unfortunately this change messed up our ability to communicate to the drone in ROS because we needed to connect to the pi through its hostname which didn't work on OLIN-DEVICES. 

### Challenges
System setup took about two weeks of time and we were often using a monitor to troubleshoot or visualize what was going on on the raspberry pi, so we didn't move outside until the very last week. We also didn't have propellers until then. During flight testing we discovered that controlling the drone to fly needed troubleshooting as it was not behaving as we had expected/hoped for. Lastly, we are still unable to echo many mavros topics from the pi. We would've liked the pi to understand the state of the drone and make it easier to debug the drone control issues. 

### Improvements
Going forward, we've built up this drone platform and we'd like to continue working with it in our final project. Some things we would like to work on are developing more control over the drone's movements, detecting and localizing the drone based on apriltags, or perhaps even adding a realsense camera.

### Lessons Learned
Autonomous flight is a difficult thing to implement and integration takes a long time. In retrospect, it would've been better to start flying the drone and working on the control and localization part earlier in the project. In general, I think we learned that we need to start flight testing and working in our intended environment earlier, and that flying outdoors is challenging because of weather (wind), lighting changes, and lack of power outlets.
