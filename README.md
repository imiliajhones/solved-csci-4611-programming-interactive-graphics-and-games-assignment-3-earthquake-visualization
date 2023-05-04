Download Link: https://assignmentchef.com/product/solved-csci-4611-programming-interactive-graphics-and-games-assignment-3-earthquake-visualization
<br>
<strong>Introduction </strong>

For this assignment, you’ll be working with data from NASA and the USGS to visualize on a globe the locations where earthquakes happened between 1905 and 2007. Visualizations incorporating geospatial data are used and analyzed in many different contexts, including navigating a city (as seen in GPS devices), commercial development, and setting governmental policy. This area also receives a significant amount of research attention. For example, Prof. Vipin Kumar and others at the University of Minnesota are working on visualizing and understanding global warming datasets as part of an NSF project (you can find more information at <u>http://climatechange.cs.umn.edu/</u>).

Your assignment should look like the image below when you’ve finished it.




The earthquake dataset you’ll be using includes 13,540 different earthquakes. The Earth texture dataset from NASA is available in resolutions down to 500m/pixel (although getting this to display on your graphics card is well beyond the scope of this assignment).

In this assignment, you will learn to:

<ul>

 <li>Visualize real-world geographical data on a 3D textured globe</li>

 <li>Apply textures to 3D objects</li>

 <li>Algorithmically create a deforming 3D mesh and display it using vertex buffers</li>

 <li>Define normal vectors and texture coordinates for a sphere</li>

 <li>Convert from spherical coordinates (latitude and longitude) to 3D Cartesian coordinates.</li>

</ul>

<h1>Earth and Earthquake Data</h1>

We have included multiple scaled-down versions of this Earth texture with the code distributed on the website, since you need a fairly powerful computer to render even the lowest-quality image from the NASA page. In order of decreasing quality, the following images are provided:

<ul>

 <li>jpg: Full-resolution (8 km/px, 5400×2700) image from NASA page</li>

 <li>earth-2k.bmp: 2048×1024 scaled-down version of image</li>

 <li>earth-1k.bmp: 1024×512 version of image</li>

 <li>earth-512.bmp: 512×256 version of image</li>

 <li>earth-256s.bmp: 256×256 version of image</li>

</ul>

Almost any graphics card should be able to use the 256s version of the Earth texture. If your computer isn’t able to use this texture, you will need to use the computers available in the various CSE Labs. The Earth textures are stored in a equirectangular projection, which simply means that the <em>x</em> coordinate corresponds directly to longitude and <em>y</em> directly to latitude.

The earthquake dataset contains information about the earthquake’s magnitude (a measure of how severe the earthquake is) and its longitude and latitude. You’ll be required to display this information on the globe in a meaningful manner. More information on the earthquakes is available in the data file, and if you are interested, you can try to figure out ways to integrate additional data variables into your visualization.

<h1>Requirements and Grading Rubric</h1>

You will be required to write the code that displays the Earth on screen and animates it between flat rectangular map and a three-dimensional globe. You will also need to write the code to display the earthquakes on the Earth. The starter code defines a single light source that coincides with the camera, so that surfaces seen head-on (i.e. whose normal points towards the viewer) will be bright while those that are slanted will be darker. Pressing Space will pause the current visualization time, while the left and right arrow keys will make it slow down and speed up respectively. Once you have the mesh being drawn, pressing M will visualize its triangles.

<em>Work in the “C” range will: </em>

Use vertex buffers and element buffers to draw a rectangle subdivided into multiple triangles, representing a flat map of the Earth. You can use the M key to view the mesh structure.

<ul>

 <li>The rectangle should lie in the <em>xy</em> plane, with x going from −<em>π</em> to <em>π</em> and <em>y</em> going from −<em>π</em>/2 to <em>π</em>/2. It must be divided into <em>slices</em> divisions horizontally and <em>stacks</em> divisions vertically. Therefore, the mesh will have (<em>slices</em>+1)×(<em>stacks</em>+1) vertices and</li>

</ul>

2×<em>slices</em>×<em>stacks</em> triangles. On the right is an example with 6 slices and 3 stacks.

<ul>

 <li>Fill in the necessary parts of the</li>

</ul>

Earth::getPosition() and Earth::getNormal() functions and use them to populate the vertex and normal arrays. You should assume that the input latitude lies between −90° to 90°, and longitude lies between −180° to 180°.

<ul>

 <li>Populate the element buffer with the appropriate vertex indices. It may help to work out the indices by hand for a small example, say <em>slices </em>= 4, <em>stacks</em> = 2, and generalize from there.</li>

</ul>

<em>Work in the “B” range will: </em>

Apply a texture of the Earth to the rectangle so that it looks like the original image.

<ul>

 <li>Remember to call unsetTexture() at the end of the Earth drawing function, so that the texture does not get applied to things drawn afterward.</li>

</ul>

Display on the Earth all the earthquakes that have happened within the past one year of the current visualization time.

<ul>

 <li>Use getPosition() to obtain the earthquake positions such that they match those of the sphere textures (i.e., an earthquake occurring in California must be displayed in the same location as the California of the Earth texture). One way to tell if your mapping is correct is if you see a lot of earthquakes along the “Ring of Fire” in the Pacific Ocean off the coast of Asia.</li>

 <li>You may use the Draw:: functions to draw the earthquake markers or come up with your own custom geometry. Earthquake sizes and colors should be based on the earthquake data in some meaningful way. Explain and justify the mapping you choose in your README.</li>

</ul>

<em>Work in the “A” range will: </em>

Change the vertex positions and normals to draw the Earth as a sphere instead of a rectangle.

<ul>

 <li>In the QuakeVis constructor, set isSpherical=1. Fill in the necessary parts of Earth::getPosition() and Earth::getNormal().</li>

 <li>You will need to convert latitude and longitude into the three-dimensional Cartesian coordinates of the corresponding point on the sphere, using the formulas <em>x</em> = cos(<em>lat</em>) sin(<em>lon</em>), <em>    y</em> = sin(<em>lat</em>), <em>    z</em> = cos(<em>lat</em>) cos(<em>lon</em>).</li>

</ul>

Be careful that the input latitude and longitude are in degrees, not radians.

<ul>

 <li>The texture is permitted to look slightly “cut off” <em>only</em> at the top and bottom stack of the Earth mesh. The image to the right shows what this looks like when <em>slices</em> = 6 and <em>stacks</em> = 3. (See below for why this happens.) Increasing the number of stacks and slices will make this problem harder to see.</li>

 <li>The earthquakes must appear on the sphere in the correct geographical locations (though they may not lie exactly on the mesh if it has too few slices and stacks).</li>

</ul>

Smoothly transform the mesh between a rectangle and a sphere based on user input. The program should start with the Earth displayed as a rectangle. When the user presses the S key, it should continuously deform to a sphere. If S is pressed again, it should deform back to a rectangle, and so on.

<ul>

 <li>To start with, you may call setSpherical() whenever the S key is pressed to update the state of the mesh.</li>

 <li>Immediately toggling the state creates an abrupt transition. Instead, it is better to smoothly transition between the rectangular and spherical shapes, by gradually changing the <em>spherical</em> parameter from 0 to 1 or vice versa over multiple frames. One way to do this is to add a variable to QuakeVis to keep track of the target value of <em>spherical</em>. Only change the target value on user input, and adjust the state of the mesh towards the target at each frame.</li>

 <li>Fill in the remainder of Earth::getPosition() and Earth::getNormal() to smoothly interpolate between the rectangular and spherical values. (Technically, the correct normal of the interpolated shape is not simply the interpolation of the normals, but we may pretend it is for this assignment.)</li>

</ul>

If you reduce the value of <em>slices</em> and <em>stacks</em> and then watch the mesh structure as it interpolates between a rectangle and a sphere, you should be able to see why the texture appears to be cut off near the poles.

<h1>Useful Math</h1>

Here are a few mathematical operations that are very common in graphics and may be useful for this assignment:

<ul>

 <li>Linear interpolation: One way to blend smoothly between two values <em>x</em> and <em>y</em> (which could be reals, or vectors, or matrices, etc.) is to define a function whose output varies continuously from <em>x</em> to <em>y</em> as a scalar parameter <em>a</em> goes from 0 to 1. This function is traditionally abbreviated “lerp”: lerp(<em>x</em>, <em>y</em>, <em>a</em>) = <em>x</em> + <em>a</em>(<em>y</em> − <em>x</em>).</li>

</ul>

Thus, for example, lerp(<em>x</em>, <em>y</em>, 0) = <em>x</em>, lerp(<em>x</em>, <em>y</em>, 1) = <em>y</em>, and lerp(<em>x</em>, <em>y</em>, ½) = (<em>x</em> + <em>y</em>)/2.

<ul>

 <li>Clamping: A concise way to constrain a value to lie in a specified interval [<em>a</em>, <em>b</em>] is to define a “clamp” function clamp(<em>x</em>, <em>a</em>, <em>b</em>) which returns <em>a</em> if <em>x</em> ≤ <em>a</em>, returns <em>b</em> if <em>x</em> ≥ <em>b</em>, and returns <em>x</em></li>

 <li>Rescaling: Suppose you have a value <em>x</em> in the range [<em>xmin</em>, <em>xmax</em>], and you want to find the corresponding value in [<em>ymin</em>, <em>ymax</em>]. Observe that <em>x</em> − <em>xmin</em> lies in [0, <em>xmax</em> − <em>xmin</em>], and (<em>x</em> − <em>xmin</em>)/(<em>xmax</em> − <em>xmin</em>) lies in [0, 1], so the desired value is <em>y</em> = <em>ymin</em> + (<em>ymax</em> − <em>ymin</em>)(<em>x</em> − <em>xmin</em>)/(<em>xmax</em> − <em>xmin</em>).</li>

</ul>

<h1>Above and Beyond</h1>

All the assignments in the course will include great opportunities for students to go beyond the requirements of the assignment and do cool extra work. We don’t offer any extra credit for this work — if you’re going beyond the assignment, then chances are you are already kicking butt in the class. However, we do offer a chance to show off… While grading the assignments the TAs will identify the best 4 or 5 examples of people doing cool stuff with computer graphics. After each assignment, the selected students will get a chance to demonstrate their programs to the class!

There are great opportunities for extra work in this assignment. For example, the source website for the Earth texture (see Data Credits below) has images for each month of the year. You could animate between the textures based upon the current time of year. We have also included some height images that contains the elevation and bathymetry data for the Earth, with sea level being 50% gray, and black and white indicating 8 km below and above sea level respectively. You could use this image to visualize the shape of the Earth’s surface by loading it with SDL_LoadBMP() and using its pixel data to displace mesh vertices along their normals. Be creative!

<h1>Support Code</h1>

The webpage where you downloaded this assignment description also has a download link for support code to help you get started. The support code for this assignment is a simple program using the SDL-based engine, similar to the ones we have used before. You should also download separately the zip file containing the texture and earthquake data files.

The support code defines a program structure and everything you need to read and parse the earthquake data file. <strong>To make locating data files simpler, we have provided a header file called </strong><strong>config.hpp that contains absolute paths to your data files. You should edit this file with the full path to your data files (e.g. </strong><strong>“C:UsersTuringa3data” or </strong><strong>“/home/turing/a3/data”).</strong> We will modify this file appropriately when grading your assignment.

<h1>Handing It In</h1>

When you submit your assignment, you should include a README file. This file should contain, at a minimum, your name and descriptions of design decisions you made while working on this project. If you attempted any “above and beyond” work, you should note that in this file and explain what you attempted.

When you have all your materials together, zip up the source files and the README, and upload the zip file to the assignment hand-in link on our Moodle site. You don’t need to include the data files, which are pretty large and which we have a copy of anyway. Any late work should be handed in the same way, and points will be docked as described in our syllabus.

<h1>Data Credits</h1>

The earthquake data comes from <u>http://earthquake.usgs.gov/data/centennial/</u>. As per <u>http://www.usgs.gov/laws/info_policies.html</u>, this data is in the public domain.

Credit: U.S. Geological Survey,

Department of the Interior/USGS

The Earth texture comes from <u>http://visibleearth.nasa.gov/view_cat.php?categoryID=1484</u>. As per <u>http://visibleearth.nasa.gov/useterms.php</u>, this data is freely available for re-use.

Credit: NASA Earth Observatory