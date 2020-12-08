# Project Final Report
## Face Mask Analysis During Covid-19 Pandemic
* **Team members**:
  * Contact person: Tianyang Zhan tzhan@andrew.cmu.edu
  * Ruoxin Huang ruoxinh@andrew.cmu.edu
  * Amanda Xu xinyix2@andrew.cmu.edu
  * Ziming He zimingh@andrew.cmu.edu
* **Track**: Model


## Introduction
In the face of the COVID-19 pandemic, it has been shown that wearing masks is an effective way to prevent the spread of the disease. CDC calls people to wear masks, and masks are required in public transportation and restaurants. Our app aims to demonstrate the effectiveness of wearing masks and show how wearing masks can increase safety levels in real-world scenarios. However, relying on manpower to check whether all people are wearing masks is cumbersome and unrealistic. Thus, it becomes an important task to automatically check whether people are wearing masks under different scenarios. Our project provides a solution by analyzing whether people in a scenario are wearing masks and providing a score to evaluate the safety level of the scene.


## Related Work
###Mask Effectiveness
As wearing masks has become a suggestive preventive method of COVID-19, researchers have analyzed the effectiveness of mask-wearing. Since transmission might happen before symptoms appear, wearing masks by well people is beneficial to lowering the risk of infection [(MacIntyre & Chughtai 2020)](https://doi.org/10.1016/j.ijnurstu.2020.103629). Analysis across different continents has also shown the universal effectiveness of mask-wearing [(Chu et.al, 2020)](https://www.researchgate.net/publication/341812100_Physical_distancing_face_masks_and_eye_protection_to_prevent_person-to-person_transmission_of_SARS-CoV-2_and_COVID-19_a_systematic_review_and_meta-analysis). Inside the US, the Delphi Group has conducted an analysis on the relationship between masks wearing and infected by Covid, and they found a strong negative relationship [(Reinhart, 2020)](https://delphi.cmu.edu/blog/2020/10/12/new-and-improved-covid-symptom-survey-tracks-testing-and-mask-wearing/).
In the visualization section, we use data collected from surveys conducted by The Delphi Research Group at Carnegie Mellon University and Facebook. These datasets include the percentage of mask-wearing and the percentage of having COVID symptoms for each state [(Delphi Research Group, 2020)](https://delphi.cmu.edu/covidcast/?sensor=doctor-visits-smoothed_adj_cli&level=county&date=20201202&signalType=value&encoding=color&mode=export&region=42003).

### Face Detection
Face detection has gained significant consideration over the years as one of the essential topics for many downstream computer vision tasks such as face recognition and image captioning. In recent computer vision research, a lot of study work was proposed in the field of Face Detection to make it more advanced and accurate. Viola and Jones proposed a  Haar-based cascade classifier to find locations of the human faces in the input image based on the assumption that human faces share some universal properties like the eyes region is darker than the neighbor pixels [(Viola & Jones, 2001)](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf). This approach is fast and computationally inexpensive but has the limitation of only able to detect objects with clear edges and lines. Thus, the algorithm is likely to fail when the faces are covered by glasses, masks, or the direction of the face is tilted to the side. Another type of Face Detection system involves more complex deep neural networks that require training on large datasets. MTCNN detector [(Zhang, et al., 2016)](https://arxiv.org/abs/1604.02878) and ResNet-based model [(He, et al. 2015)](https://arxiv.org/abs/1512.03385) allow us to draw a bounding box around partially obscured faces at the cost of computational time.

### Mask Detection
With the COVID-19 pandemic, the branch of Face Mask Detection has gained more focus. Most related research work and open-source projects formulate this task as an image classification problem, where the input is an image containing a single human face, and the output is a binary label of "with_mask" or "without_mask". To enable real-time image and video analysis, some systems are built on top of the MobileNetV2 deep neural network [(Sandler, et al. 2019)](https://arxiv.org/abs/1801.04381) that achieves the state-of-the-art performance of mobile models on multiple computer vision tasks and benchmarks.


## Methods
In the visualization part of our project, we wish to demonstrate the effectiveness of face masks. We do so by plotting the percentage of the population having COVID symptoms against the percentage of the population wearing masks in each state. First, we create a scatter plot where each point is a state and drew the best fit line to demonstrate the correlation. This plot is created using Streamlit functions and is interactive in that it allows the users to move the plot around and choose the desired timeframe and the state they are interested in. Next, we present two map plots of the USA on the same data to strengthen our claim. The first map visualization colors each state by the percentage of people not wearing masks, while the second plot colors each state by the percentage of people having COVID symptoms. The two maps use identical color schemes to convey the correlation. These plots are produced using Altair plots as they are not directly producible using Streamlit. 

To accurately detect all masked and unmasked faces in the input image, we need to combine a Face Detection model with a Mask Detection model into a single pipeline. First, we use the Face Detection model to detect and select bounding boxes B_i around all faces in the input image I. Then for each selected bounding box, we perform image transformation to extract and resize the faces as the input image for the Mask Detection model for classification. Based on the output label and confidence score of the Mask Detection model, we add different colors and text to the bounding boxes on the original image I to generate an output image O. We choose the pre-trained Caffe-based ResNet-10 face detector from the [OpenCV library](https://github.com/opencv/opencv/tree/master/samples/dnn/face_detector) as our Face Detection Model. To reduce computational latency and to support future real-time analysis extensions, we use a MobileNetV2 based face mask classifier from an [open-source project](https://github.com/ikigai-aa/Face-Mask-Detector-using-MobileNetV2) pretrained on an [face mask image dataset](https://drive.google.com/drive/folders/1XDte2DL2Mf_hw4NsmGst7QtYoU7sMBVG?usp=sharing) of 3835 real-world images of masked and unmasked faces.


## Results
In the visualization section, the best fit line of the scatter plot clearly demonstrates a positive correlation between wearing masks and reduced covid symptoms. The color schemes in the map plots match very closely, implying that in states where people do not wear masks, the chances of having Covid symptoms is higher. Both results show that wearing masks is effective in mitigating the spread of the virus.


## Discussion
The visualization part involves the percentage of mask-wearing and having covid symptoms in each state. The interactive elements enable users to investigate data themselves and have a deep understanding of the negative relationship between wearing masks and the spread of the COVID-19 virus. In this way, users will learn the importance of wearing masks in preventing the spread of COVID-19.

In the face mask detection part, users can upload an image and obtain a safety score. The calculation process of the score is transparent to users since users can see mask detection results and distance measure, tune the threshold values, and review the explanations of results. By knowing factors influencing the score, users will better understand the relationship between wearing masks, maintaining social distance, and getting infected. Users are free to choose parameters of face detection threshold and distance scale so that calculations are personalized. With this app, users can know the safety level of different scenarios by uploading an image of the scene to get a score. Consequently, users can avoid unsafe scenes and go to safer ones to protect themselves and control the spread of the disease.


## Future Work 
There are several limitations to our current work that we want to improve in future work. In the visualization part, we can incorporate data with a larger time span and finer granularity. Specifically, we can collect the symptom data for each county in the US and make the geographic visualization plot more interactive. 

For the mask detection part, we can experiment with different datasets, models, training setups to get better detection accuracy. We also notice some inaccurate distance estimation for faces along the z-dimension of the image. In future work, we can also experiment with complex machine learning models to get more accurate estimations.

Due to the time constraint on this project, we narrow down the scope of the app from real-time video analysis to single image analysis. But our selected model fully satisfies the latency requirement for video processing. In future work, we can extend the app to support static image and video analysis as well as real-time video analysis. This can expand the use case of our app and allow us to develop mobile apps and embedded software for public surveillance systems to monitor and evaluate COVID safety levels in various locations.

## Reference
1. Raina MacIntyre, C., & Chughtai, A.A. (2020). A rapid systematic review of the efficacy of face masks and respirators against coronaviruses and other respiratory transmissible viruses for the community, healthcare workers and sick patients. International Journal of Nursing Studies,108. https://doi.org/10.1016/j.ijnurstu.2020.103629.
2. Chu, D. K., Akl, E. A., Duda, S., Solo, K., Yaacoub, S., Schünemann, H. J., & COVID-19 Systematic Urgent Review Group Effort (SURGE) study authors (2020). Physical distancing, face masks, and eye protection to prevent person-to-person transmission of SARS-CoV-2 and COVID-19: a systematic review and meta-analysis. Lancet (London, England), 395(10242), 1973–1987. https://doi.org/10.1016/S0140-6736(20)31142-9
3. Reinhart, A. (2020, Oct 12). New and Improved COVID Symptom Survey Tracks Testing and Mask-Wearing. Retrieved from https://delphi.cmu.edu/blog/2020/10/12/new-and-improved-covid-symptom-survey-tracks-testing-and-mask-wearing/ 
4. Delphi Research Group. (2020). Delphi Survey Results. Retrieved from https://delphi.cmu.edu/covidcast/?sensor=doctor-visits-smoothed_adj_cli&level=county&date=20201202&signalType=value&encoding=color&mode=export&region=42003 