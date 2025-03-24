# Face Morphing

By: Catherine Wang

## Introduction:

In this project, we will be playing with images’ domains to create face morphing effects from one person to another. In addition, I will compute the mean of a population of faces and extrapolate from a population mean to create a caricature of myself, how fun! So in order to do that, there are a few intermediate steps. In order for the face morphing to actually ‘make sense’ and not just be a change in the individual pixels, we need to define some correspondences in facial features to keep consistent between the two images, and then from there we can perform Delaunay triangulation to get some very nice not-weirdly-skinny triangles. From there, we can go through the triangles and manipulate the two human face images to actually start morphing. So to recap, a morph consists of a simultaneous warp of the image shape and a cross-dissolve of the image colors. A cross-dissolve is just a weighted average of individual pixel intensities of the two images, but a warp of shape is more involved and we must use correspondences between images.

## Part 1: Defining Correspondences

The two faces/images that I wanted to morph are my favorite middle aged white men, Cillian Murphy and Benedict Cumberbatch, and here are the two images of them I will be using to perform everything else below on.

![Cillian Murphy](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cillianresized.jpg)

Cillian Murphy

![Benedict Cumberbatch](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/benedictresized.jpg)

Benedict Cumberbatch

So now that we know we need facial correspondence at the key features, I went ahead and manually selected using this [tool](https://inst.eecs.berkeley.edu/~cs194-26/fa22/upload/files/proj3/cs194-26-aex/tool.html) 52 points for each image, making sure that the order was the same. I also made sure to include the four corners of the images as that would ensure it is part of our warp and so we don’t get a weird cutout that only warps their faces and leaves the rest of the image just black (trust me I did that first). So with that done, I simply calculated the midpoints between the two images’ points and performed Delaunay triangulation on that, which gave me a nice mesh upon which to perform some morphing on!

![trianglemesh.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/trianglemesh.jpg)

## Part 2: **Computing the "Mid-way Face"**

Now before we actually create the morph animation in the next step, I will compute the ‘midway face’ of these images, which involves computing the average shape, warping each of the original two faces into that shape, and then finally just averaging the colors together as well. To actually perform the warp, we will be using an affine warp for each triangle in the triangulation from the original images into this new shape, which meant computing an affine transformation matrix between many sets of two triangles, for each of the the midway Delaunay triangulation’s triangles and its corresponding triangle mapped onto one of the two faces. This affine transformation was in charge of finding the correct corresponding source pixels from the original image. The inverse affine transformation is used so that each pixel in the average face shape image has a source (with no holes).

In order to get the midway image, we needed to warp each of the people into their midway shape first, which the result of is displayed below:

![Warped Cillian](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cillianwarped.jpg)

Warped Cillian

![Warped Benedict](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/benedictwarped.jpg)

Warped Benedict

Below is the original two images on either side and the mid-way face in the middle!

![Cillian](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cillianresized%201.jpg)

Cillian

![[**♡♡♡](https://fsymbols.com/heart/)** MIDWAY FACE [**♡♡♡**](https://fsymbols.com/heart/)](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/midface.jpg)

[**♡♡♡](https://fsymbols.com/heart/)** MIDWAY FACE [**♡♡♡**](https://fsymbols.com/heart/)

![Benedict](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/benedictresized%201.jpg)

Benedict

## Part 3: The Morph Sequence

Now we have an animated sequence of Cillian morphing into Benedict. 

![morphseq.gif](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/morphseq.gif)

## Part 4: **The "Mean face" of a population**

Then we will compute the ‘mean face’ of a specific population. For me, I used the dataset of [Danish faces](https://web.archive.org/web/20210305094647/http://www2.imm.dtu.dk/~aam/datasets/datasets.html) to compute the mean face of that entire population. There three main steps for doing that are:

1) Compute the average face shape of the whole population. Once I did that I calculated the Delaunay triangulation on that average face shape and here is what the triangle mesh looked like:

![danes_triangle_mesh.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/danes_triangle_mesh.jpg)

2) Warp each of the faces in the dataset into the average shape using this triangulation above. Here are some examples of the individual results:

![dane14warped.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/dane14warped.jpg)

![dane25warped.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/dane25warped.jpg)

![dane21warped.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/dane21warped.jpg)

![dane30warped.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/dane30warped.jpg)

3) Compute the average face of the population. Here is the average Danish face based on this dataset. We can see it looks like a male because in this dataset, they are much more populated

![mean_danes_face.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/mean_danes_face.jpg)

Here is an image of me! Now I incorporate myself into this dataset by:

![cath_resized.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cath_resized.jpg)

1) Creating an image of my face warped into the average geometry

![cath_as_dane.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cath_as_dane.jpg)

2) Creating an image of the average face warped into my geometry

![dane_as_cath.jpg](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/dane_as_cath.jpg)

![Catherine as a Danish Man in Full Morph (Shape and Appearance)](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/danishmancath_fullmorph.jpg)

Catherine as a Danish Man in Full Morph (Shape and Appearance)

## **Part 5: Caricature, Extrapolating from the mean**

Now I am going to produce a caricature of my face by extrapolating from the population mean we calculated in the last step. Thus, it highlights the features in my face that are different/missing in the Danish dataset and warps it accordingly. From this we can see that compared to the Danes, my eyes are more angled upwards and theirs downwards (thus this dropping accentuation). Moreover it seems the Danes have a more heart-shaped forehead while mine is just an oval, and thus in this caricature I get some angled horn features that make my forehead look coffin shaped. Moreover we can see that the Danes have more arched eyebrows and relatively speaking more downward sloping lips (also because I was smiling a bit).

![Catherine Caricature (Based on Danish average)](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cath_caricature.jpg)

Catherine Caricature (Based on Danish average)

## Bells and Whistles #1

Now I will change the gender and ethnicity of my face. I will try and change into the average French man based on a pre-computed mean image from the internet. I first made sure to resize both images so facial features are relatively in the right area/aligned (mainly with eyes and face width), and that the images are the same shape/size.

![Mean Frenchman](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/frechman_resized.jpg)

Mean Frenchman

![Catherine!!](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cath_resized2.jpg)

Catherine!!

Now let’s show morphing just the shape, just the appearance, and both.

![Shape Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/frenchmancath_shapemorph.jpg)

Shape Morph

![Appearance Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/frenchmancath_appearancemorph.jpg)

Appearance Morph

![Full Morph (Shape and Appearance)](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/frenchmancath_fullmorph.jpg)

Full Morph (Shape and Appearance)

## Bells and Whistles #2

This is a creation of option 2: Make a morphing music video on a theme. The theme is Friends in Cory :((

[https://youtube.com/shorts/4hY8bU927dQ?feature=share](https://youtube.com/shorts/4hY8bU927dQ?feature=share)

## Bells and Whistles #3: Catherine as Women of the World

After I made me into the French man, I really wanted to see me as other ethnicities as well so decided to make the following morphs, where I do full morphs of myself into the average English woman, Mexican woman, Indian woman, and Ethiopian woman. This is basically what I would look like not if I was just directly a lady of that ethnicity, but rather if I had a female child with each of these respective average ladies.

![Mean English](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/english.jpg)

Mean English

![Mean Mexican](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/mexican.jpg)

Mean Mexican

![Mean Indian](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/indian.jpg)

Mean Indian

![Mean Ethiopian](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/ethiopian.jpg)

Mean Ethiopian

![English Woman Full Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/englishcath_fullmorph.jpg)

English Woman Full Morph

![Mexican Woman Full Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/mexicancath_fullmorph.jpg)

Mexican Woman Full Morph

![Indian Woman Full Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/indiancath_fullmorph.jpg)

Indian Woman Full Morph

![Ethiopian Woman Full Morph](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/ethiopiancath.jpg)

Ethiopian Woman Full Morph

### Catherine Morphing into Women of the World!!

Now for the video of me morphing through all these results! This keeps looping through seamlessly, starting with:

- normal me → English → Mexican → Indian → Ethiopian

![cath_ethnicities.gif](Face%20Morphing%200b168dfd720b4375be585ff6ede90d81/cath_ethnicities.gif)