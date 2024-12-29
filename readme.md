<h1><strong>Self-Driving Car with Pixy Camera, ROS2, Computer Vision, and Machine Learning</strong></h1>
<p>This project implements an autonomous self-driving car system utilizing a <strong>Pixy Camera</strong> for vision-based navigation and control. The car autonomously traverses a track containing various obstacles such as <strong>under- and over-bridges</strong>, <strong>zig-zag paths</strong>, and <strong>speed bumps</strong>. The project integrates <strong>ROS2</strong> for control and <strong>Gazebo</strong> for simulation in an <strong>Ubuntu 20.04</strong> environment. Additionally, the system uses <strong>Machine Learning (ML)</strong> and <strong>Computer Vision (CV)</strong> techniques for dynamic environment perception and decision-making.</p>
<hr>
<h2><strong>Project Overview</strong></h2>
<p>The primary objective of this project is to autonomously navigate a car along a track, using the <strong>Pixy Camera</strong> for real-time vision processing. The track has several challenges, including:</p>
<ul>
<li><strong>Over-bridge and under-bridge navigation</strong></li>
<li><strong>Zig-zag turns</strong></li>
<li><strong>Speed bumps</strong></li>
</ul>
<p>The car relies on a combination of <strong>computer vision</strong> to identify the track and <strong>machine learning models</strong> to handle various complex scenarios like traffic signs and dynamic road conditions.</p>
<hr>
<h2><strong>Final Track Path</strong></h2>
<p><img src="https://user-images.githubusercontent.com/79053599/222254405-9e1ae793-0220-495e-ba96-97befae539ba.png" alt="Final Track"></p>
<p><img src="https://github.com/user-attachments/assets/35a64ac6-7fb8-4460-a671-a0d6d96385e9" alt="Track Image 1"></p>
<p><img src="https://github.com/user-attachments/assets/8f77a880-1e40-4052-95bc-3c70b359924f" alt="Track Image 2"></p>
<hr>
<h2><strong>System Architecture Overview</strong></h2>
<p>This system is composed of several key layers:</p>
<ol>
<li>
<p><strong>Computer Vision (CV) Layer</strong>:<br>
This layer is responsible for processing images captured by the Pixy Camera. It detects the track's boundaries, traffic signs, and other critical features.</p>
</li>
<li>
<p><strong>Machine Learning (ML) Layer</strong>:<br>
The ML layer is used to process traffic sign data and adjust the car’s behavior based on real-time road signs and environmental changes.</p>
</li>
<li>
<p><strong>Line Following and Control Layer</strong>:<br>
This layer interprets the data from the vision system and controls the car's steering, speed, and movement decisions.</p>
</li>
</ol>
<hr>
<h2><strong>Installation Instructions</strong></h2>
<p>To run this project on your system, follow the steps below:</p>
<ol>
<li>
<p><strong>Install ROS2 and Required Packages</strong><br>
Follow the installation guide for <strong>ROS2</strong> and other dependencies <a href="https://nxp.gitbook.io/nxp-aim" title="https://nxp.gitbook.io/nxp-aim" target="_blank" rel="noopener noreferrer" class="px-1">here</a>.</p>
</li>
<li>
<p><strong>Clone the Repository</strong><br>
Clone this GitHub repository into your workspace:</p>
<pre>git clone https://github.com/Pranay-Pandey/self-driving-car.git</pre>

</li>
<li>
<p><strong>Install Dependencies</strong><br>
Install all the required packages as outlined in the <a href="https://nxp.gitbook.io/nxp-aim/installation-of-nxp-gazebo" title="https://nxp.gitbook.io/nxp-aim/installation-of-nxp-gazebo" target="_blank" rel="noopener noreferrer" class="px-1">NXP Gitbook</a>.</p>
</li>
<li>
<p><strong>Replace the Script Files</strong><br>
Replace the following files in your ROS2 workspace with the ones from this repository:</p>
<ul>
<li><code>aim_line_follow.py</code> → <code>ros2ws/src/aim_line_follow/aim_line_follow.py</code></li>
<li><code>nxp_track_vision.py</code> → <code>ros2ws/src/nxp_cup_vision/nxp_cup_vision.py</code></li>
</ul>
</li>
<li>
<p><strong>Launch the Simulation</strong><br>
To start the simulation, use the following command:</p>
  <pre>ros2 launch sim_gazebo_bringup sim_gazebo.launch.py</pre>
</li>
</ol>
<hr>
<h2><strong>Technical Details and Algorithm Breakdown</strong></h2>
<h3><strong>Computer Vision (CV) Layer</strong></h3>
<p>The <strong>CV layer</strong> processes the input from the Pixy Camera to detect road boundaries and track features. It extracts crucial data points from images and sends these to the steering and control system.</p>
<h4><strong>Track Boundary Detection (nxp_track_vision.py)</strong></h4>
<p>The main script for track boundary detection is <code>nxp_track_vision.py</code>. This script processes the image captured by the Pixy Camera and extracts <strong>two vectors</strong> representing the left and right edges of the track. The algorithm works as follows:</p>
<ol>
<li>
<p><strong>Image Preprocessing</strong>:<br>
The image captured by the Pixy Camera is preprocessed to enhance features relevant to road detection, such as color segmentation and edge detection.</p>
</li>
<li>
<p><strong>Vector Extraction</strong>:<br>
Two vectors are calculated to represent the track’s left and right boundaries. This is done using geometric methods that identify the positions of white pixels (representing the road) in the image.</p>
</li>
<li>
<p><strong>Track Boundary Adjustment</strong>:</p>
<ul>
<li>If both vectors are on the same side of the track center, the car adjusts its steering to move away from that side.</li>
<li>The system also checks for the car’s position relative to <strong>over-bridges</strong>, adjusting the steering mechanism when crossing these areas using the calculated slope of the detected vectors.</li>
</ul>
</li>
<li>
<p><strong>Vector Steering Adjustment</strong>:<br>
The steering angle is derived from the average slope of the two vectors, ensuring the car follows the track accurately. The system also adjusts for changes in the Y-values of the vectors, particularly useful for steering over bridges.</p>
</li>
</ol>
<h4><strong>Traffic Sign Detection and Processing</strong></h4>
<p>Traffic sign recognition is an essential part of this project. Using <strong>machine learning (ML)</strong> techniques, the system can identify and interpret road signs, adjusting the car’s behavior accordingly.</p>
<ul>
<li>
<p><strong>Traffic Sign Dataset</strong>:<br>
A custom-trained model is used to detect traffic signs. The model is trained on a set of predefined signs (e.g., stop signs, speed limits).</p>
</li>
<li>
<p><strong>Sign Processing</strong>:<br>
The car identifies the presence of traffic signs by analyzing the pixels in the captured image. If a sign is detected, the relevant action (e.g., stop or slow down) is triggered.</p>
</li>
</ul>
<hr>
<h3><strong>Machine Learning (ML) Layer</strong></h3>
<p>The <strong>ML layer</strong> is responsible for enhancing the car’s decision-making capabilities, specifically related to traffic sign recognition and adaptive behavior.</p>
<h4><strong>Traffic Sign Recognition</strong></h4>
<ol>
<li>
<p><strong>Model Overview</strong>:<br>
The system uses a <strong>Convolutional Neural Network (CNN)</strong> trained to recognize various traffic signs. The training dataset includes images of common road signs, such as <strong>Stop</strong>, <strong>Speed Limit</strong>, <strong>Pedestrian Crossing</strong>, and more.</p>
</li>
<li>
<p><strong>Sign Detection Process</strong>:<br>
Once the Pixy Camera captures an image, the ML model processes the image to detect and classify any road signs present. The car then adjusts its behavior based on the identified sign. For instance:</p>
<ul>
<li><strong>Stop Sign</strong>: The car will stop if it detects a stop sign.</li>
<li><strong>Speed Limit Sign</strong>: The car will adjust its speed according to the sign's speed limit.</li>
</ul>
</li>
</ol>
<h4><strong>Adaptive Decision-Making with ML</strong></h4>
<p>In addition to traffic sign recognition, the ML system also adjusts the car’s behavior in response to complex road conditions or obstacles. By continually learning from the environment, the system becomes more adaptive and responsive.</p>
<hr>
<h3><strong>Line Following and Control Layer</strong></h3>
<p>The <strong>control layer</strong> processes the output from the computer vision and machine learning layers to control the car’s steering and speed.</p>
<ol>
<li>
<p><strong>Steering Control (aim_line_follow.py)</strong>:<br>
The <code>aim_line_follow.py</code> script receives steering commands from the vision system, specifically from the left and right pixel density calculations. The system works by:</p>
<ul>
<li>Calculating the <strong>left and right pixel densities</strong> within a specific image window.</li>
<li>Determining the car’s steering angle based on these densities to ensure smooth, accurate turns.</li>
</ul>
</li>
<li>
<p><strong>PID Control</strong>:<br>
A <strong>PID controller</strong> is applied to the pixel density values to prevent the car from making rapid or erratic movements. The formula for steering adjustment is as follows:</p>
<pre>self.steer_vector.z = self.steer_vector.z + (pid(L) - pid(R))/2.0</pre>
</li>
<li>
<p><strong>Speed Adjustment</strong>:<br>
To ensure the car moves at an optimal speed, the system also adjusts the velocity in response to varying terrain conditions (e.g., uphill or downhill):</p>
<pre>self.speed_vector.x = self.speed_vector.x + kp * error</pre>
<p>Here, <code>error</code> is the difference between the desired and actual car velocities, and <code>kp</code> is a constant that adjusts the car’s speed.</p>
</li>
<li>
<p><strong>Over-Bridge Handling</strong>:<br>
The system uses a state variable (<code>state == 1</code>) to detect when the car is on the bridge. During bridge crossings, the steering and speed are adjusted to maintain stability.</p>
</li>
</ol>
<hr>
<h2><strong>Video Demonstrations</strong></h2>
<ul>
<li>
<p><strong>Final Track Demo</strong>:<br>
Watch the car navigating the final track:</p>
<p><a href="https://github.com/Pranay-Pandey/self_driving_car/blob/main/SDC.mp4" title="https://github.com/Pranay-Pandey/self_driving_car/blob/main/SDC.mp4" target="_blank" rel="noopener noreferrer" class="px-1"><img src="https://user-images.githubusercontent.com/79053599/179050684-9dd6d71e-123a-48b2-b03d-dd4d4a5c2c73.mp4" alt="Final Track Demo"></a></p>
</li>
<li>
<p><strong>Car Navigation Video</strong>:<br>
Watch a full demonstration of the car navigating the track:</p>
<p><a href="https://github.com/Pranay-Pandey/self_driving_car/blob/main/Self_driving_car.mp4" title="https://github.com/Pranay-Pandey/self_driving_car/blob/main/Self_driving_car.mp4" target="_blank" rel="noopener noreferrer" class="px-1"><img src="https://github.com/Pranay-Pandey/Self-driving-car/assets/79053599/16f33848-bc43-47aa-83f8-541c1f5aadc1" alt="Car Navigation Demo"></a></p>
</li>
</ul>
<hr>
<h2><strong>Conclusion</strong></h2>
<p>This project integrates <strong>Computer Vision</strong>, <strong>Machine Learning</strong>, and <strong>ROS2</strong> to create an autonomous self-driving</p></div>
