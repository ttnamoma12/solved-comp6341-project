Download Link: https://assignmentchef.com/product/solved-comp6341-project
<br>



<h1>DESCRIPTION</h1>

In this assignment, you will write software which stitches images together to form panoramas. Your software will detect useful features in the images, find the best matching features in your other images, align the photographs, then warp and blend the photos to create a seamless panorama.

This assignment can be thought of as two major components:

<ol>

 <li>Feature Detection and Matching [Assignment #2]</li>

 <li>Panorama Mosaic Stitching [<u>this assignment</u>]</li>

</ol>

<h1>FEATURE DETECTION AND MATCHING [Steps 1 and 2] – assignment 2</h1>

<strong>Step 1:</strong> Compute the Harris corner detector using the following steps

<ol>

 <li>Compute the x and y derivatives on the image</li>

 <li>Compute the covariance matrix H of the image derivatives. Typically, when you compute the covariance matrix you compute the sum of I<sub>x</sub><sup>2</sup>, I<sub>y</sub><sup>2</sup> and I<sub>x</sub>I<sub>y</sub> over a window or small area of the image. To obtain a smooth result for better detection of corners, use a Gaussian weighted window.</li>

 <li>Compute the Harris response using determinant(H)/trace(H).</li>

 <li>Find peaks in the response that are above the threshold, and store the interest point locations.</li>

</ol>




<strong>Required:</strong> Open “Boxes.png” and compute the Harris corners. Save an image “1a.png” showing the Harris response on “Boxes.png” (you’ll need to scale the response of the Harris detector to lie between 0 and 255. ) When you’re debugging your code, I would recommend using “Boxes.png” to see if you’re getting the right result.




Compute the Harris corner detector for images “Rainier1.png” and “Rainier2.png”. Save the images

“1b.png” and “1c.png” of the detected corners (use the drawing function cv::circle(…) or cv::drawKeypoints(…) to draw the interest points overlaid on the images)




<strong>Step 2:</strong> Matching the interest points between two images.

<ol>

 <li>Compute the descriptors for each interest point.</li>

 <li>For each interest point in image 1, find its best match in image 2. The best match is defined as the interest point with the closest descriptor (SSD or ratio test). C. Add the pair of matching points to the list of matches.</li>

 <li>Display the matches using cv::drawMatches(…). You should see many correct and incorrect matches.</li>

</ol>




<strong>Required:</strong> Compute the Harris corner detector and find matches for images “Rainier1.png” and “Rainier2.png”. Save the image “2.png” showing the image with the found matches [use cv::drawMatches(…) to draw the matches on the two image].




<h1>PANORAMA MOSAIC STITCHING [Steps 3 and 4] – this assignment</h1>

<strong>Step 3:</strong> Compute the homography between the images using RANSAC (Szeliski, Section 6.1.4). Following these steps:

<ol>

 <li>Implement a function project(x1, y1, H, x2, y2). This should project point (x1, y1) using the homography “H”. Return the projected point (x2, y2). Hint: See the slides for details on how to project using homogeneous coordinates. You can verify your result by comparing it to the result of the function cv::perspectiveTransform(…). [Only use this function for verification and then comment it out.].</li>

 <li>Implement the function computeInlierCount(H, matches, numMatches, inlierThreshold). computeInlierCount is a helper function for RANSAC that computes the number of inlying points given a homography “H”. That is, project the first point in each match using the function “project”. If the projected point is less than the distance “inlierThreshold” from the second point, it is an inlier. Return the total number of inliers.</li>

 <li>Implement the function RANSAC (matches , numMatches, numIterations, inlierThreshold, hom, homInv, image1Display, image2Display). This function takes a list of potentially matching points between two images and returns the homography transformation that relates them. To do this follow these steps:

  <ol>

   <li>For “numIterations” iterations do the following:

    <ol>

     <li>Randomly select 4 pairs of potentially matching points from “matches”.</li>

     <li>Compute the homography relating the four selected matches with the function cv::findHomography(…)***. Using the computed homography, compute the number of inliers using “computeInlierCount”.</li>

    </ol></li>

  </ol></li>

</ol>

<ul>

 <li>If this homography produces the highest number of inliers, store it as the best homography.</li>

</ul>

<ol>

 <li>Given the highest scoring homography, once again find all the inliers. Compute a new refined homography using all of the inliers (not just using four points as you did previously. ) Compute an inverse homography as well, and return their values in “hom” and “homInv”.</li>

 <li>Display the <u>inlier</u> matches using cv::drawMatches(…).</li>

</ol>

<strong>Required:</strong> Compute the Harris corner detector, find matches and run RANSAC for images “Rainier1.png” and “Rainier2.png”. Save the image “3.png” of the found matches (use cv::drawMatches(…) to show the inlier matches). You should only see correct matches, i.e. , all the incorrect matches from the previous step should be removed. If you see all or some incorrect matches try running RANSAC with a larger number of iterations. You may try tuning the other parameters as well.

<strong>***NOTE: The function cv::findHomography(…) can optionally use RANSAC internally, if that is enabled. </strong><strong>You should <u>not</u>  have RANSAC enabled in this function i.e. third input parameter should be 0.</strong>




<strong>Step 4:</strong> Stitch the images together using the computed homography. Following these steps:

<ol>

 <li>Implement the function stitch(image1, image2, hom, homInv, stitchedImage). Follow these steps:</li>

</ol>

<ol>

 <li>Compute the size of “stitchedImage. ” To do this project the four corners of “image2” onto “image1” using project(…) and “homInv”. Allocate the image.</li>

 <li>Copy “image1” onto the “stitchedImage” at the right location.</li>

 <li>For each pixel in “stitchedImage”, project the point onto “image2”. If it lies within image2’s boundaries, add or blend the pixel’s value to “stitchedImage. ” When finding the value of image2’s pixel use bilinear interpolation [cv::getRectSubPix(…)].</li>

</ol>




<strong>Required:</strong> Compute the Harris corner detector, find matches, run RANSAC and stitch the images “Rainier1.png” and “Rainier2.png”. Save the stitched image as “4.png”. It should look like the image

“Stitched.png”.








