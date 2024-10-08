# Computer Vision Project Report

> **Installation:** Installation guidance provided in the original repository and remains the same.

## Task
To reduce the training complexity of the splatter-model, enhancing its ability to generalize effectively with less data.

## Explanation
We applied a method to decrease training time, resulting in a less complex training process. This reduction in complexity aims to support the generalization of the model, enabling it to perform well with a smaller dataset.

## Methodology

### Data Preparation
We utilized a pretrained splatter-image model to extract depth maps from each image in the dataset. These depth maps served as ground truth labels for training. Our hypothesis was that using depth maps in this way would yield promising results while consuming fewer computational resources.

> **Note:** If we had access to a dataset with depth maps extracted under more controlled or optimal conditions, the results would likely have been even better.

### Data Re-arrangement
In this step, we ensured that each depth map was properly aligned with its corresponding RGB input image, enabling accurate training inputs.

### Training Process
Due to hardware limitations, we focused on training the model to learn only the geometric structure of **cars**, using a reduced amount of data. To optimize the training process, we adjusted both the training procedure and the model inputs. 

<p align="center">
  <img src="results/architecture.png" alt="Training Architecture">
  <br>
  <em>Architecture diagram showing the training process with RGB input and depth map</em>
</p>

Specifically, during training, we fed the model the depth map of each image along with the corresponding RGB image. By using the depth map as ground truth for (x, y, z) reconstruction, the model was able to achieve more accurate geometric structure reconstruction of the 3D model without needing to predict it from scratch. This approach resulted in a faster understanding of the data, leading to a more efficient training process.

In parallel, we also trained the original network on the same data but without the depth map for comparison purposes. This allowed us to evaluate the impact of including depth maps on the model's performance and efficiency.

## Results

<p align="center">
  <img src="results/graphs_first_stage.png" alt="Stage one results">
  <br>
  <em>Stage one results: Performance metrics comparing our model (purple) with the original model (orange) across different evaluation metrics</em>
</p>

<p align="center">
  
| Metric         | Original Model | Our Model | Improvement |
|----------------|----------------|-----------|-------------|
| LPIPS_cond     | 0.1284         | 0.0699    | 45.56%      |
| LPIPS_novel    | 0.2188         | 0.1500    | 31.44%      |
| PSNR_cond      | 25.8992        | 28.7034   | 10.83%      |
| PSNR_novel     | 18.6321        | 20.6051   | 10.59%      |
| SSIM_cond      | 0.9297         | 0.9648    | 3.77%       |
| SSIM_novel     | 0.8285         | 0.8756    | 5.68%       |
| Training Loss  | -2.2634        | -2.4195   | 6.89%       |

</p>

<p align="center">
  <img src="results/graphs_second_stage.png" alt="Stage two results">
  <br>
  <em>
    Stage two results: Performance metrics comparing our model (pink) with the original model (orange) across different evaluation metrics</em>
</p>

<p align="center">  

| Metric               | Original Model | Our Model | Improvement |
|----------------------|----------------|-----------|-------------|
| LPIPS_cond           | 0.0866         | 0.0379    | 56.24%      |
| LPIPS_novel          | 0.1955         | 0.1350    | 30.94%      |
| PSNR_cond            | 27.9042        | 30.3524   | 8.77%       |
| PSNR_novel           | 18.3668        | 20.5007   | 11.62%      |
| SSIM_cond            | 0.9553         | 0.9762    | 2.19%       |
| SSIM_novel           | 0.8291         | 0.8759    | 5.64%       |
| Training Loss        | -2.2886        | -2.4609   | 7.53%       |
| Training L12 Loss    | -2.4185        | -2.5913   | 7.14%       |
| Training LPIPS Loss  | -0.8640        | -1.0346   | 19.75%      |

</p>

### Analysis
As we can see, our model consistently outperforms the original in all the key areas we measured. The results show clear improvements in image quality, with higher PSNR values, especially when generating images based on specific conditions. The lower LPIPS scores indicate that the images our model generates are perceptually closer to the actual ground truth, and the higher SSIM scores suggest that our model preserves structural details more effectively. 

We can observe that both our model and the original model show slightly lower values in the PSNR_cond graph. This is because, in the second stage, we continue training with an additional loss that corresponds to enhancing the 3D-realistic representation, which slightly lowers PSNR values but improves the structural accuracy of the model.

The training loss graph shows that our model trains more efficiently and stays more stable, keeping loss values lower throughout. These combined improvements across various metrics suggest that our model is significantly better at both conditional and novel image generation compared to the original baseline.

### Key Capabilities

1. **Conditional Image Generation:** The model generates images based on given inputs or conditions (e.g., labels, partial images), producing outputs that match the specific input requirements.

2. **Novel Image Generation:** The model creates new, unseen images without explicit conditioning, demonstrating its ability to generate high-quality, diverse images independently of input data.

## Reconstruction Results

<p align="center">
<em>Order of reconstruction results: Ours, Original, Ground Truth</em>
</p>

<p align="center">
  <img src="results/ours.gif" alt="ours">
  <img src="results/original.gif" alt="original">
  <img src="results/gt.gif" alt="gt">
  <br>
</p>

<p align="center">
  <img src="results/ours_2.gif" alt="ours">
  <img src="results/original_2.gif" alt="original">
  <img src="results/gt_2.gif" alt="gt">
  <br>
  <em>Figure: Multiple views of reconstructed car models demonstrating consistent quality across different perspectives</em>
</p>
