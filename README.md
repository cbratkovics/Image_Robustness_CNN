# Robustness Analysis – Christopher Bratkovics

## Introduction
For my final project, I analyzed the robustness of four popular pre-trained image classification models against various perturbations. The four models included in my analysis are **VGG16, ResNet50, DenseNet121, and EfficientNetB0**.  

Since it's well known that pre-trained image classification models aren't typically robust to image perturbations, the goal of my analysis was to **evaluate each model’s prediction consistency** when faced with perturbations expected in real-world scenarios.  

**Prediction consistency** in this context refers to how often a model’s prediction of an original image remains unchanged when perturbations are applied. Additionally, I analyzed **feature map similarity** between the original and perturbed images using **Cosine and Euclidean distance metrics**.

---

## **Original Image vs. Perturbed Images**
### **Original Image**
<img src="./images/justice_league.png" alt="Original Justice League Image" width="500"/>

### **Image with Perturbations Applied**
<img src="./images/perturbated_images.png" alt="Perturbed Images" width="800"/>

---

## Analysis & Implementation
While the four selected models differ significantly in architecture, they share key characteristics that allowed for consistent evaluation:
- Input shape of **224x224** pixels
- **Pre-trained weights** on the ImageNet dataset
- Comparable **Top-5 accuracy** on the ImageNet validation dataset
- The same **input preprocessing requirements**  

Despite their shared characteristics, I specifically chose these models for their **architectural diversity**:
- **VGG16** – Deep convolutional neural network
- **ResNet50** – Deep residual learning framework
- **DenseNet121** – Dense connectivity pattern
- **EfficientNetB0** – Optimized for efficiency & performance  

### **Perturbations Applied**
To test model robustness, I applied the following **10 perturbations** to the base image:

1. **Increased brightness**
2. **Increased contrast**
3. **Gaussian blur**
4. **Defocus blur**
5. **Fog**
6. **90-degree rotation**
7. **Horizontal flip**
8. **Vertical flip**
9. **Random noise**
10. **Salt and pepper noise**  

These transformations were implemented using the **Pillow (PIL) library** for image manipulation.

### **Implementation Details**
I created a `ptb_apply` function that:
- Applies each perturbation to the input image
- Saves the modified images to a list for further analysis  

This function ensures **reproducibility** by allowing flexibility to change the base image while maintaining consistent perturbation techniques.

---

## **Prediction Consistency Evaluation**
To evaluate model robustness, I initialized the four models with **pre-trained ImageNet weights** and:
1. **Generated baseline predictions** for the original image  
2. **Applied perturbations and checked if predictions changed**  
3. **Calculated a prediction consistency score**:
   \[
   \text{Prediction Consistency} = \frac{\text{# of consistent predictions}}{\text{Total Perturbations (10)}}
   \]

### **Feature Similarity Metrics**
To further analyze robustness, I computed **Cosine and Euclidean distance metrics** to compare feature maps of the original image and its perturbed versions:
- **Cosine Similarity** – Measures the cosine of the angle between feature vectors (scale-invariant)  
- **Euclidean Distance** – Measures the actual straight-line distance between vectors, normalized for interpretation  

I implemented:
- `extract_features()` – Extracts feature maps for an image  
- `cosine_eval()` – Computes Cosine similarity  
- `euclidean_eval()` – Computes Euclidean similarity  

---

## **Results & Conclusion**
### **Prediction Consistency Scores**
| Model          | Consistency Score (%) |
|---------------|----------------------|
| **DenseNet121**  | **90%**  |
| **VGG16**        | 40%  |
| **EfficientNetB0** | 20%  |
| **ResNet50**     | 0%   |

**Key Takeaways:**
- **DenseNet121** performed the best, with **90% consistency** (9 out of 10 predictions remained unchanged).  
- **ResNet50 failed completely** (0% consistency), meaning it changed predictions for **every perturbation**.  

### **Feature Similarity Metrics**
| Model          | Average Cosine Similarity | Average Euclidean Similarity |
|---------------|--------------------------|------------------------------|
| **DenseNet121**  | **0.65**  | **0.94**  |
| **VGG16**        | 0.62  | 0.91  |
| **ResNet50**     | 0.47  | **0.95**  |
| **EfficientNetB0** | 0.27  | 0.85  |

**Findings:**
- **DenseNet121 had the highest robustness**, supported by **high prediction consistency (90%) and strong feature similarity scores**.  
- **ResNet50 had the highest Euclidean similarity (0.95) but a 0% consistency score**, indicating that while its feature maps were stable, its **predictions changed drastically**.  
- **EfficientNetB0 performed the worst in terms of feature similarity and prediction consistency**, suggesting it is the least robust model in this test.

### **Final Conclusion**
**DenseNet121** is the most robust image classification model among the four analyzed.  
It achieved:
✔ **Highest prediction consistency (90%)**  
✔ **Highest average Cosine similarity (0.65)**  
✔ **Strong Euclidean similarity (0.94)**  

This suggests **DenseNet121 is the most resistant** to real-world image perturbations.

---

## **Issues Faced**
During this analysis, I encountered a few challenges:
1. **Model Selection** – Finding diverse models that allowed for a fair comparison  
2. **Perturbation Choices** – Balancing the number of perturbations while ensuring meaningful results  
3. **Single Image Limitation** – Using only one image for evaluation may limit generalizability  

Future work could involve:
- Expanding the dataset with multiple test images  
- Testing additional perturbations  
- Evaluating models trained on **adversarial robustness techniques**  

---

## **Repository & Code**
🔗 **[GitHub Repository](https://github.com/cbratkovics/robustness-analysis)**  
The full implementation, including model evaluation and perturbation functions, is available in the repository.

---

📌 **Created by Christopher Bratkovics** | Hosted on **GitHub Pages**
