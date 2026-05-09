### Testing the model on 300 frames and stitched those frames into a single video. Check it out::
  https://youtu.be/cq_YnqT9YLI?si=8mMgwOHpZfZOUDrL
  

###                                                   Person Re-ID with YOLOv8m and BoT-SORT for Dynamic Camera Systems
A robust tracking pipeline optimized for cross-camera identity consistency using Medium-scale YOLOv8 and motion-compensated tracking.


###                                                                               Inspiration

*In real-world surveillance and person re-identification (Re-ID), the primary obstacle isn't just detecting a person,
  but maintaining a consistent identity across a "blind" network of multiple cameras.
*The ReID-X challenge provides a complex environment where traditional trackers often fail due to viewpoint changes and frequent occlusions.
*My motivation was to build a system that bridges the gap between raw object detection and stable temporal association,
  ensuring that a single global ID follows an individual through the entire sequence.

  

###                                                                           What the project does

This project implements a high-performance computer vision pipeline for the Computer Vision / ReID-X Track.
Detection: It identifies individuals across seven synchronized cameras with high precision.

Tracking: It assigns a persistent pred_track_id to each detected person.

Re-Identification: The system is designed to handle "ID flickering" and maintain identity consistency even 
when individuals disappear behind obstacles or transition between different camera angles.

Output: Generates a standardized submission.csv containing localized bounding boxes and associated track IDs for over 119,000 test frames.



###                                                                             How it was built

The system architecture follows a "Tracking-by-Detection" philosophy:

Model Backbone: I utilized YOLOv8m (Medium). I chose the Medium variant over the Nano baseline because it contains significantly more parameters (approx. 26M),
allowing for deeper feature extraction which is vital for distinguishing individuals with similar clothing or in low-light conditions.

Tracking Algorithm: I integrated the BoT-SORT tracker. BoT-SORT was specifically selected for this "Dynamic Camera" competition because it incorporates Camera Motion Compensation (CMC).
This ensures that the tracker doesn't lose an ID simply because the camera itself is moving or zooming.

Optimization: The inference is performed at a 640px resolution to balance the Detection Accuracy requirements with the execution time limits of the Kaggle environment.



###                                                                          Challenges I ran into

Resource Constraints: Processing 119,631 frames is a massive computational task. I initially experimented with higher resolutions (1024px) and Test-Time Augmentation (TTA), 
but had to optimize the code to prevent GPU Quota exhaustion and System RAM crashes.

Association Gaps: Maintaining IDs when a person exits one camera's view and enters another remains the hardest part of the HOTA metric. 
Fine-tuning the conf (confidence) and iou thresholds was necessary to reduce false positives that disrupt association accuracy.


###                                                                     Accomplishments that I am proud of

I am proud of achieving a stable 1st place rank on the public leaderboard during the development phase. 
Successfully deploying a YOLOv8m + BoT-SORT pipeline that processes the entire test set without hitting the 12-hour timeout was a significant technical milestone.

###                                                                             What I learned
I gained understanding of the HOTA (Higher Order Tracking Accuracy) metric. I learned that simply having "good boxes" Detection Accuracy isn't enough; 
true success in Re-ID comes from Association Accuracy — the ability of the model to maintain the mathematical "link" between frames.

###                                                                        Next steps for my project

If given more time, I will try to:
Implement a dedicated Re-ID Feature Gallery to store appearance embeddings of people who have left the frame.



