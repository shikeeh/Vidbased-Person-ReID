# Vidbased-Person-ReID
Selection and implementation of a video-based person re-identification algorithm on a single camera.

## 1. Introduction
Because this is a newer concept to me, I will attempt to clearly define this project. 
The task given to me was to implement a video-based person re-identification algorithm on a single camera as part of a hiring assessment. I was allowed to implement models already built instead of having to build my own.
- Person re-identification (reID) aims to retrieve person videos with the same identity as a query person across non-overlapping cameras (simply find, but the task is not so straightforward for a machine)
- This project will focus on the video-based setting as compared to an image-based setting that identifies people with still images. Thus it aims at matching the video tracklets with cropped video frames for identifying the pedestrians under different cameras. Possible reasons include
    - Video sequences are more practical for real-world scenarios, and
    - The appearance information with spatial and temporal relations in a video tracklet contains more cues for matching people under different views
- To implement the algorithm on single camera is defined to test the model with data from a single camera. This is not to be confused with single-camera-training (SCT) setting (Zhang et al. 2019), which *trains* the model on data from multiple cameras, but each person only appears in 1 camera and not in any others. 

## 2. Choice of Model
### 2.1 Overview of Models
In my research, I had to understand what were some common methods and issues faced when attempting to implement a video-based person re-identification algorithm. Especially in recent years, there have been many different methods created, some novel and some not, to tackle issues such as spatial and temporal memory, scalability, applicability, etc. Some notable papers have attempted unsupervised learning (Chen et al. 2018, Zang et al. 2021), omni-scale feature learning (Zhou et al. 2019), global-guide reciprocal learning (Liu et al. 2021), gait recognition (Zhang et al. 2022), and others.

### 2.2 Model Choice
Ultimately, the model that I have chosen to implement comes from the paper "Video-based Person Re-identification without Bells and Whistles" (Liu et al. 2021). I will be using the code from its repository (link in references). This model is perhaps the most straightforward supervised learning method to improve performance. It introduces two key ideas:
1. A re-Detect and Link (DL) module which effectively crops tracklets, reducing unexpected noise
2. An improved model called Coarse-to-Fine Axial-Attention Network (CF-AAN) to improve efficiency by reducing computation burden while retaining most of its performance

This model was chosen over others after comparing the ideas behind different methods and their effectiveness. This model can be used as a new baseline for supervised learning models, particularly because of the DL module which works to effectively re-centre the image. After applying this module, future models can include other methods such as those listed above (omni-scale feature learning, global-guide reciprocal learning) which work to effectively identify smaller but just as important clues (eg. keychain on a backpack, shoes, etc.)

### 2.3 Limitations of Model
However, this model requires much more data preparation and cleaning in order to be able to utilize the extra information. When it comes to the Re-ID phase, the cost of the DL module is not felt as the model has already been trained on the cleaned data. However, this means that this method is not feasible on unsupervised models unless data preparation is performed by applying the module on all data to be fed to the unsupervised model first.

## 3. Comparison with FairMOT and DeepSORT
Because of time constraint, this section will be brief. It seems that FairMOT is actually a relatively new method known as one-shot trackers. Unlike Person Re-IDs, FairMOT trains its own private detectors. DeepSORT is an older method based on the SORT method, and seems to struggle more against denser images (images with much higher human traffic) as compared to Person Re-ID methods (Ishikawa et al. 2021). 

## Additional Comments
Some struggles, takeaways, and questions I had/have, but insufficient time to answer:
1. I struggled in implementing the model on a single camera. The repository was not intuitive to follow and clone to be deployed on my chosen host, Streamlit. This will likely result in me being unable to implement and complete the question.
2. I also struggled with rigorously understanding the many papers I read through. Below are only a few of those that I found, and the rest were not added due to relevance to the main model chosen. It regrettably slowed down my own efficiency, but I did enjoy learning something new.
3. I was also caught off-guard at the scale of the assessment, much to my regret. I initially thought the 24 hours was given just as a deadline to complete a typical assessment that would last 30 minutes to 2 hours at most, and had other appointments planned during the day. At the time of writing this, I am unconfident of being able to properly present my attempt for question 2. Nonetheless, I am very thankful to have been able to take part in such an assessment for the first time, and I did love the entire process.
4. A part of me questions if I made the right choice choosing this question instead of the video-based action recognition question, given the stricter parameters this question was framed in, and that a preliminary search on action recognition even yielded an entire Medium article dedicated to a step-by-step guide to implementing one. But the grass is always greener on the other side, and I personally found this question more intriguing and immediately practical.
5. After researching, I am still unsure of the differences between omni-scale feature learning and global-guide reciprocal. They seem to be trying to accomplish the same thing, but through different means. A question I have that can probably be answered in time is, how different are these two methods, and is one definitively better than the other?
6. One takeaway is that unsupervised learning for Person Re-ID seems to be an newer field that is gaining pace because it is much more scalable (which is much needed as we increase the number of cameras). I would expect unsupervised learning models to be able to compete or even overtake performance of supervised models in the near future.

## References
1. We referred to this GitHub repository: https://github.com/jackie840129/CF-AAN
2. Ishikawa, H., Hayashi, M., Phan, T. H., Yamamoto, K., Masuda, M., & Aoki, Y. (2021). Analysis of Recent Re-Identification Architectures for Tracking-by-Detection Paradigm in Multi-Object Tracking. In VISIGRAPP (5: VISAPP) (pp. 234-244).
3. Liu, C., Chen, J., Chen, C., & Chien, S. (2021). Video-based Person Re-identification without Bells and Whistles. 2021 IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW), 1491-1500.
4. Liu, X., Zhang, P., Yu, C., Lu, H., & Yang, X. (2021). Watching You: Global-guided Reciprocal Learning for Video-based Person Re-identification. 2021 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 13329-13338.
5. Zhang, T., Xie, L., Wei, L., Zhang, Y., Li, B., & Tian, Q. (2020). Single Camera Training for Person Re-identification. ArXiv, abs/1909.10848.
6. Zhang, S., Wang, Y., Chai, T., Li, A., & Jain, A.K. (2022). RealGait: Gait Recognition for Person Re-Identification. ArXiv, abs/2201.04806.
7. Zhou, K., Yang, Y., Cavallaro, A., & Xiang, T. (2019). Omni-Scale Feature Learning for Person Re-Identification. 2019 IEEE/CVF International Conference on Computer Vision (ICCV), 3701-3711.
