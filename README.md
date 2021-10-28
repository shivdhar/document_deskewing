# Document Deskewing

## Task

Given an arbitrary image containing text, rotate it such that the text is horizontal and the right side up.

## Approach

### Conventional Approaches

- One way to deskew documents is to use the Hough transform.
The process is:
    - read an RGB image
    - convert to grayscale
    - apply Canny/Sobel filters for edge detection
    - apply the Hough transform to detect lines
    - given the lines, calculate the orientation of the longest lines.

- The FFT algorithm can be also be used. We apply it in [00_fft.ipynb](00_fft.ipynb).
The process is 

- The basic idea is to obtain the orientation of text characters/words/sentences/blocks/paragraghs
- Compare with the previous approaches of getting the orientation of the "image" as opposed to the text within it.
- To do this, we need the bounding boxes of the text characters.

## References and Further Reading

- https://github.com/zacharywhitley/awesome-ocr
- https://becominghuman.ai/how-to-automatically-deskew-straighten-a-text-image-using-opencv-a0c30aed83df
- https://medium.com/@9sphere/machine-vision-recipes-deskewing-document-images-e178278- https://www.pyimagesearch.com/2017/02/20/text-skew-correction-opencv-python/94c34

