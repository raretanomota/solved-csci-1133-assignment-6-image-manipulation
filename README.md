Download Link: https://assignmentchef.com/product/solved-csci-1133-assignment-6-image-manipulation
<br>
<h1></h1>

Every image rendered on a computer can fundamentally be represented as a three dimensional matrix.  An image can be broken down into two dimensional array of pixels (short for “picture element”), which appear to be tiny squares of a single color that, when combined, make up the images you see on a computer screen.  However, this isn’t quite accurate: each pixel is actually displayed as light produced by three tiny light-emitting diodes: one red, one green, and one blue.







Image credit: ​<u><a href="https://en.wikipedia.org/wiki/LED_display">https://en.wikipedia.org/wiki/LED_display</a></u>




For this reason, pixel data is itself stored as a list of three elements, representing the relative strengths of the red, green, and blue diodes needed to produce the required color for the pixel; these are often given as numbers between 0 and 255.  Since images are two dimensional arrays of pixels, and we can represent a pixel as a one-dimensional list of three integers, we can represent an image as a three dimensional matrix (that is, a list of lists of lists) of integers in Python.  In the following problems, you will be manipulating matrices of this format in order to alter image files in various ways.  You can see an example of how an image is represented as a 3D matrix below:




<table width="0">

 <tbody>

  <tr>

   <td width="120">[255, 128, 50</td>

  </tr>

 </tbody>

</table>

[[​[0, 0, 255]​, [255, 255, 255]],

[​[255, 0, 0]​, ​[0, 255, 0]​],

[​[0, 255, 255]​, ​​]​],

<h2>[​[127, 127, 127]​, ​[0, 0, 0]​]]</h2>




You may notice a few oddities in this representation.  First, note that the pixels are listed starting with the bottom row, meaning that if you print out each row of the matrix on a different line as above, the layout almost matches the image, except that it’s flipped vertically.  Second, the channels (color components) for each pixel are listed in the order [blue, green, red] rather than what you may be familiar with, the standard RGB.  The reason that we’re choosing this representation is because it matches the order in which the data is listed within the .bmp files that we’ll be using (see <u><a href="https://en.wikipedia.org/wiki/BMP_file_format">https://en.wikipedia.org/wiki/BMP_file_forma</a></u><u>​        </u><u><a href="https://en.wikipedia.org/wiki/BMP_file_format">t</a></u>​ for more info), but thankfully, for the tasks that you’ll be completing, neither of these oddities really matter: none of the tasks you will be facing require you to know what order the pixels are listed in or what order the colors are within each pixel.




<strong>Download the template file </strong>​<strong>hw6.py</strong>​<strong> from Canvas, along with all of the example .bmp files (They </strong>

<strong>are all in the compressed folder labelled </strong>​<strong>hw6.zip)</strong>​​. The template file has a function

transform_image(fname, operation)

which is designed to read in the bmp format image file at fname, convert the data into a pixel matrix as described above, perform an operation on it, and then convert that matrix back into .bmp file.  In particular, the input file is assumed to be in the same directory as hw6.py, and the output file will be

named the same as the input, except with &lt;​operation&gt;_ appended to the front.  For example,​        transform_image(‘cat.bmp’, ‘grayscale’)​ will produce a grayscale version of cat.bmp​, called ​grayscale_cat.bmp​.




This function is already working, so you do not need to edit it.  However, the function itself doesn’t actually perform any operations on the pixel matrix generated from the .bmp file; it instead calls on one of four unimplemented functions to do that work: ​invert​, ​grayscale​, ​rotate​, and ​edge_detect​.




All of these functions take in a three dimensional matrix that represents an image, as described above, and should return another matrix representing the image with the specified operation performed.  It is up to you to decide whether to create a new matrix, or just alter the original to accomplish this.  Your task in this assignment is to implement each operation.




<strong>Constraints</strong>​:

<ul>

 <li>Do not import/use any Python modules except for ​copy​ (see part C hints).</li>

 <li>Do not alter the transform_image​ or ​ big_end_to_int​ functions in the hw6.py template.​             Do not change the function names or arguments.</li>

 <li>Follow the instructions in the ​py​ template.</li>

 <li>Your submission should have no code outside of the function definitions (comments are fine). You are permitted to write helper functions outside of the ones provided in the template.</li>

 <li>You are not required to push any of the .bmp files to github: only the ​py​ file will be graded.</li>

</ul>







Problem A. Color Inversion

First, you will be implementing the invert​ ​ function.  (Reminder: all of these functions take in a three-dimensional list, and return a three-dimensional list: it’s up to you whether to create a new list or alter the original).  This function performs a color inversion, as though you were producing a negative of a photo.  Recall that each color component in every pixel is an integer between 0 and 255, inclusive.  In every pixel, for each of the three color components, if the original value for the component was ​x​, to invert the color you must set it to ​255-x​.




<strong>Examples</strong>​:




&gt;&gt;&gt; invert([[​[0, 0, 255]​, [255, 255, 255]],

[​[255, 0, 0]​, ​[0, 255, 0]​],

<table width="0">

 <tbody>

  <tr>

   <td width="120">[255, 128, 50</td>

  </tr>

 </tbody>

</table>

[​[0, 255, 255]​, ​​]​],

<h2>[​[127, 127, 127]​, ​[0, 0, 0]​]])</h2>




[[​[255, 255, 0]​, ​[0, 0, 0]​],

[​[0, 255, 255]​, ​[255, 0, 255]​],

[​[255, 0, 0]​, ​[0, 127, 205]​],

[​[128, 128, 128]​, [255, 255, 255]]]




<em>(note that the highlighting and the new line for each row are for clarity purposes only and will not be present in the actual console output) </em>




Below is the result of using ​transform_image​ with the ​invert​ operation on the following files found on Canvas, assuming that they are downloaded to the same folder as your ​hw6.py​ script:







&gt;&gt;&gt; transform_image(‘dice.bmp’,’invert’)

&gt;&gt;&gt; transform_image(‘cat.bmp’,’invert’)

&gt;&gt;&gt; transform_image(‘connector.bmp’,’invert’)




dice.bmp                            invert_dice.bmp

(source: ​<u><a href="https://en.wikipedia.org/wiki/Dice">https://en.wikipedia.org/wiki/Dice</a></u>​)

cat.bmp                            invert_cat.bmp

connector.bmp                       invert_connector.bmp







<h2>Problem B. (10 ​<strong>Grayscale</strong></h2>

Next, you will be implementing the grayscale​ ​ function.  This converts the image to grayscale (a black and white image).  To do this, for every pixel, compute the average (mean) of the three color components, rounding down, and then set all three components to that value.




(Note: This isn’t exactly the way real grayscale conversion works, but it’s close enough.  See <u><a href="https://en.wikipedia.org/wiki/Grayscale">https://en.wikipedia.org/wiki/Grayscale</a></u>​ ​or <u><a href="https://www.tutorialspoint.com/dip/grayscale_to_rgb_conversion">https://www.tutorialspoint.com/dip/grayscale_to_rgb_conversion</a></u><u>​</u>if you’re interested in why this is technically wrong).




<strong>Examples</strong>​:




&gt;&gt;&gt; grayscale([[​[0, 0, 255]​, [255, 255, 255]],

[​[255, 0, 0]​, ​[0, 255, 0]​],

[​[0, 255, 255]​, ​[255, 128, 50]​],

<h3>[​[127, 127, 127]​, ​[0, 0, 0]​]])</h3>




[[​[85, 85, 85]​, [255, 255, 255]],

[​[85, 85, 85]​, ​[85, 85, 85]​],

[​[170, 170, 170]​, ​[144, 144, 144]​],

<h3>[​[127, 127, 127]​, ​[0, 0, 0]​]]</h3>







Below is the result of using ​transform_image​ with the ​grayscale​ operation on the following files found on Canvas, assuming that they are downloaded to the same folder as your ​hw6.py​ script:







&gt;&gt;&gt; transform_image(‘dice.bmp’,’grayscale’)

&gt;&gt;&gt; transform_image(‘cat.bmp’,’grayscale’)

&gt;&gt;&gt; transform_image(‘connector.bmp’,’grayscale’)




dice.bmp                           grayscale_dice.bmp

cat.bmp                            grayscale_cat.bmp

connector.bmp                       grayscale_connector.bmp










<h2>Problem C. (10 ​<strong>Rotate</strong></h2>

Next, you will be implementing the rotate​ ​ function.  This rotates the image by 180 degrees (which is equivalent to reflecting it about both the horizontal axis and the vertical axis).




<strong>Hints:  </strong>

<ul>

 <li>A pixel at location (x,y) in the original matrix should end up at location (width-x-1,height-y-1) in the output matrix, assuming that we start counting at 0 (see diagram below).</li>

 <li>For this problem and (especially) the next one, you may find it easier if you can create a copy of the pixel matrix to avoid the problem of overwriting pixel data in the original matrix before you’re done using them to compute a pixel in the final matrix. Since this is a nested list structure, the easiest way to do this is to ​import copy​ and then use the <u>​</u><u><a href="https://docs.python.org/3.7/library/copy.html">deepcopy</a></u>​ method.</li>

</ul>










<strong>Examples</strong>​:




&gt;&gt;&gt; rotate([[​[0, 0, 255]​, [255, 255, 255]],

[​[255, 0, 0]​, ​[0, 255, 0]​],

[​[0, 255, 255]​, ​[255, 128, 50]​],

[​[127, 127, 127]​, ​[0, 0, 0]​]])




<h3>[[​[0, 0, 0]​, ​[127, 127, 127]​],</h3>

[​[255, 128, 50]​, ​[0, 255, 255]​],

[​[0, 255, 0]​, ​[255, 0, 0]​],

[[255, 255, 255], [0​ , 0, 255]]]​







Below is the result of using ​transform_image​ with the ​rotate​ operation on the following files found on Canvas, assuming that they are downloaded to the same folder as your ​hw6.py​ script:




&gt;&gt;&gt; transform_image(‘dice.bmp’,’rotate’)

&gt;&gt;&gt; transform_image(‘cat.bmp’,’rotate’)

&gt;&gt;&gt; transform_image(‘connector.bmp’,’rotate’)




dice.bmp                             rotate_dice.bmp

cat.bmp                              rotate_cat.bmp

connector.bmp                       rotate_connector.bmp







<h1>Problem D. (10 ​Edge Detection Filter</h1>

Finally, you will be implementing the ​edge_detect​ function.  This task is a bit more involved than any of the previous ones, so you will definitely want to write the function incrementally (that is, write some of the functionality, test that, and then only move on when you’re sure the first part works).




We want to highlight pixels that are significantly brighter (in at least one of the color components) than their surrounding pixels.  A sudden shift in brightness like this is bound to occur along any sharp visible edge.  To accomplish this, we’ll take each pixel, multiply its color values by 8, and then subtract the color values of the 8 pixels that surround it on the grid.  Pixels that are very similar to their surrounding pixels will then end up with a color close to black, since 8 times the color component of that pixel minus the color components of its 8 neighbors should end up around 0.




This means that for a pixel located at (x,y) in the original image with original red color value r(x,y), you can compute the red component for the pixel the new image r’(x,y) with the following formula:




r’(x,y) = 8*r(x,y) – r(x-1,y-1) – r(x,y-1) – r(x+1,y-1) – r(x-1,y) – r(x+1,y) – r(x-1,y+1) – r(x,y+1) – r(x+1,y+1)




A similar formula applies for the green and blue components.  Note that this may produce a value that is above the maximum color component value of 255: if this occurs then you should just set the value to 255.  Similarly, if the formula generates a negative number, just set the value to 0.




<u>For pixels along the boundaries of the picture that do not have 8 neighbors, ignore the formula and set</u> <u>their pixel values to black ([0, 0, 0]), to avoid the issue of going out of bounds.</u>




<strong>Hints:  </strong>

<ul>

 <li>See the previous problem’s hint about creating a copy of pixel matrix so that you don’t have to worry about overwriting values before you can use them to compute their neighbors.</li>

 <li>You may want to consider writing a helper function that computes the new pixel value for a single pixel, and then use that function in a nested loop inside ​edge_detect​. This will make it easier to test whether that part of your implementation works before moving on.</li>

</ul>




<strong>Examples</strong>​:

&gt;&gt;&gt; edge_detect([[​[0, 0, 255]​, [255, 255, 255]],

[​[255, 0, 0]​, ​[0, 255, 0]​],

[​[0, 255, 255]​, ​[255, 128, 50]​],

<h2>[​[127, 127, 127]​, ​[0, 0, 0]​]])</h2>




[[​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​], ​[0, 0, 0]​]]




<em>(Note: none of the above pixels have 8 neighbors, so they are all set to black)</em>

&gt;&gt;&gt; edge_detect(

[[​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​],

[​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​],

[​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[47,198,255]​],

[​[47,198,255]​,​[47,198,255]​,​[47,198,255]​,​[131,38,79] ​,​[131,38,79] ​],

[​[47,198,255]​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​],

[​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​],

[​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​],

[​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​,​[131,38,79] ​]])

[[​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 160, 176]​, ​[0, 255, 255]​, ​[0, 0, 0]​],   [​[0, 0, 0]​, ​[0, 255, 255]​, ​[0, 255, 255]​,   ​[255, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​,   ​[255, 0, 0]​,   ​[168, 0, 0]​,    ​[84, 0, 0]​, ​[0, 0, 0]​],   [​[0, 0, 0]​,    ​[84, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 0, 0]​],   [​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​,     ​[0, 0, 0]​, ​[0, 0, 0]​]]

<em>(spacing adjusted in the above example to clarify pixel structure) </em>




&gt;&gt;&gt; edge_detect([[​[0,0,10]​, ​[0,0,20]​, ​[0,0,30]​, ​[0,0,200]​],

[​[255,120,20]​, ​[70,120,30]​, ​[0,120,40]​, ​[0,0,60]​],

[​[0,120,30]​, ​[255,120,40]​, ​[0,120,60]​, ​[0,120,50]​],

[​[255,120,40]​, ​[255,120,60]​, ​[255,120,50]​, ​[255,120,40]​]])




[[​[0, 0, 0]​, ​[0, 0, 0]​, ​[0, 0, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​, ​[50, 255, 0]​, ​[0, 255, 0]​, ​[0, 0, 0]​],

[​[0, 0, 0]​, ​[255, 0, 0]​, ​[0, 120, 110]​, ​[0, 0, 0]​],

[​[0, 0, 0]​, ​[0, 0, 0]​, ​[0, 0, 0]​, ​[0, 0, 0]​]]




Below is the result of using ​transform_image​ with the ​edge_detect​ operation on the following files found on Canvas, assuming that they are downloaded to the same folder as your ​hw6.py​ script. Note that these may take significantly longer to run than the previous functions, due to the complexity of the operation (for reference, each required about 15 seconds on my laptop).










&gt;&gt;&gt; transform_image(‘dice.bmp’,’edge_detect’)

&gt;&gt;&gt; transform_image(‘cat.bmp’,’edge_detect’)

&gt;&gt;&gt; transform_image(‘connector.bmp’,’edge_detect’)




dice.bmp                          edge_detect_dice.bmp

cat.bmp                           edge_detect_cat.bmp

connector.bmp                   edge_detect_connector.bmp





