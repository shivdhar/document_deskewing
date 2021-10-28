# Document Deskewing

## Task

Given an arbitrary image containing text, rotate it such that the text is both horizontal and the "right side up".

## Approach

### Conventional Approaches

- One way to deskew documents is to use the Hough transform.
The process is:
    - read an RGB image
    - convert to grayscale
    - apply Canny/Sobel filters for edge detection
    - apply the Hough transform to detect lines
    - given the lines, calculate the orientation of the longest lines.

- The FFT algorithm can be also be used. We apply it in [00_fft.ipynb](00_fft.ipynb). Here, we find the orientation of a center line generated by the transform ,and rotate it accordingly.

These geometry-based techniques suffer from the flaw that while they understand lines, they do not understand text and its orientation. Hence, a fully flipped image remains so, because its lines are not skewed.

To solve this, we try to look at techniques which can recognize *text* instead of lines and detect their skewness accordingly.

We looked at PaddleOCR which is near the top of the [Papers With Code leaderboard for scene text detection](https://paperswithcode.com/task/scene-text-recognition). However, it only has a CLI-based interface which draws the bounding boxes in the image directly. More crucially, it failed to detect text in flipped and rotated documents.

This was a good opportunity to explore Google Cloud's AI Vision APIs. We tried it and it gave remarkably accurate results, even for documents which had been completely flipped. Most importantly, the vertices it returns for bounding boxes were ordered, starting from the upper left corner of a text segment, ensuring that no matter what the orientation of the text, the correct angle could be calculated.

- We try this approach in [04_gcp_vision.ipynb](04_gcp_vision.ipynb) and it gave remarkable results. Please see the results at the end of the notebook. Note: To run the notebook, an API key needs to be generated and placed at `gcp_credentials.json`.

### Approach

#### **See [04_gcp_vision.ipynb](04_gcp_vision.ipynb) for implementation and results.**

- The basic idea is to obtain the orientation of text characters/words/sentences/blocks/paragraghs
- Compare with the previous approaches of getting the orientation of the "image" as opposed to the text within it.
- To do this, we need the bounding boxes of the text characters. This can be provided by GCP (as here) or with some other component, probably a neural network.


### Results

- We were able to deskew all test images with a reasonable degree of precision.
- This approach worked well even in cases where conventional line-detection approaches failed.

### Further Work

- The current output can be postprocessed with Hough to straighten the images with greater precision.
- Given a dataset of correctly-aligned images, we can apply enough skewing/noising/other transforms to create a **self-supervised task** of predicting correct orientation, replacing the entire approach with a single model with image input and image/angle output.

## References and Further Reading

### Papers

- Huang K, Chen Z, Yu M, Yan X, Yin A. An Efficient Document Skew Detection Method Using Probability Model and Q Test. Electronics. 2020; 9(1):55. https://doi.org/10.3390/electronics9010055

- Baviskar, D., Ahirrao, S., Potdar, V., & Kotecha, K. (2021). Efficient Automated Processing of the Unstructured Documents Using Artificial Intelligence: A Systematic Literature Review and Future Directions. IEEE Access, 9, 72894-72936.

- Boiangiu, C., Dinu, O., Popescu, C., Constantin, N.V., & Petrescu, C. (2020). Voting-Based Document Image Skew Detection. Applied Sciences, 10, 2236.

- Song, W., Zhu, J., Li, Y., & Chen, C. (2016). Image Alignment by Online Robust PCA via Stochastic Gradient Descent. IEEE Transactions on Circuits and Systems for Video Technology, 26, 1241-1250.


### Blogs

- https://github.com/zacharywhitley/awesome-ocr
- https://becominghuman.ai/how-to-automatically-deskew-straighten-a-text-image-using-opencv-a0c30aed83df
- https://medium.com/@9sphere/machine-vision-recipes-deskewing-document-images-e178278- https://www.pyimagesearch.com/2017/02/20/text-skew-correction-opencv-python/94c34
