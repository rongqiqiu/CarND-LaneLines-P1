# **Finding Lane Lines on the Road** 

## Pipeline

The pipeline consists of 6 steps:

1. Convert the images to grayscale;
2. Apply Gaussian blur with kernel size of 5;
3. Apply Canny edge detection with low threshold of 150 and high threshold of 200;
4. Apply a polygonal image mask with the following vertices: (0, h), (w * .45, h * .6), (w * .55, h * .6), (w, h);
5. Apply Hough transform to detect lines;
6. Draw detected lines and merge into original images.

In order to draw a single line on the left and right lanes, the draw_lines() function is re-implemented as follows:

1. For each line segment (x1, y1) -- (x2, y2) (assuming y1 < y2), compute the slope as (x2 - x1) / (y2 - y1);
2. Extrapolate the line to the boundary of ROI. Denote the two endpoints as (x1', y1') -- (x2', y2'), where y1' = h * .6, y2' = h;
3. Check if abs(slope) is within [45 deg, 75 deg]; if not, skip the current line segment;
4. If slope < 0, add two endpoints to lists left_1s, left_2s, respectively; if slope > 0, add two endpoints to lists right_1s, right_2s, respectively;
5. Compute the medians of each of the four lists, and draw (up to) two lines with these endpoints.

## Shortcomings

1. When the lanes are not prominent enough from the background (e.g., in the shadow, occluded), the canny detector fails to detect them;

2. When applying the pipeline to a video, detected lanes "jump" between adjacent frames, because detection is performed frame by frame with no temporal information. 

## Improvements

1. Parameters of canny detector may be tuned to accommodate such cases;

2. Temporal information may be introduced to smooth out results.
