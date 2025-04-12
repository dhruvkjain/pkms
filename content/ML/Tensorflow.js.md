[[ML/index|ML/index]]

# Tensorflow.js : 
https://m.youtube.com/playlist?list=PLOU2XLYxmsILr3HQpqjLAUkIPa5EaZiui

 >Narrow AI : trained to do specific thing like or even better than humans.

> Machine Learning : An approach to AI where system learns from patterns.

> Deep Learning : A technique to implement Machine Learning.

> Reinforcement Learning : try to take actions to maximize its reward.

> Transfer Learning : retrain existing models with new data.

## How to train ML model :
- Features and Attributes (color shape size weight position etc.)
- Visualizing features
- Choose an algorithm

## TensoFlow.js :
Tensorflow.js is high level Layers API (like Keras(high level layers API for python)) which was build after deeplearn.js which was a low level mathematical Ops API which required more knowledge of ML and Math to run a model on browser.

> Both Python and JavaScript code for Tensorflow.js are simply build on top of C++ core with the help of C/C++ bindings.

Models in Tensorflow.js can run on both client and server side.
Client side our hardware have many options and execution time of ML model is based on hardware of client.
While on server side hardware is fix and mostly of better quality and better scalability options

> Client Side hardware options :
![[Pasted image 20240809150913.png]]

## Tensors :
There are 6 dimensions / rank 6 tensors supported in Tensorflow.js
if you have 4 values in an array then an vector is 4d but tensor is 1d with 4 values in it therefore tensor is of rank 1.

- **0 dimensions / Rank 0 tensor :**
	```javascript
	// vanilla js
	let values = 6;
	
	// tensorflow.js
	let tensor = tf.scalar(6)
	```
- **1 dimension / Rank 1 tensor :** (Example: a single coordinate in 3d space)
	```javascript
	// vanilla js
	let values = [1,2,3];
	
	// tensorflow.js
	let tensor = tf.tensor1d([1,2,3]);
	```
- **2 dimension / Rank 2 tensor :** (Example: a 2 dimensional grayscale image (0-255))
	```javascript
	// vanilla js
	let values = [
		[2,0,67],
		[0,7,5],
		[9,5,25]
	][]()
	
	// tensorflow.js
	let tensor = tf.tensor2d([
		[2,0,67],
		[0,7,5],
		[9,5,25]
	]);
	```
	The shape of the tensor for a grayscale image is typically : 
	`(Height, Width) = 0-255 value`
	```python
	import matplotlib.pyplot as plt
	import tensorflow as tf
	
	# grayscale_image_tensor = tf.zeros((256, 256), dtype=tf.uint8);
	# image_array = grayscale_image_tensor.numpy()
	
	image_tensor = tf.random.uniform((256, 256), minval=0, maxval=255, dtype=tf.int32)
	image_array = image_tensor.numpy()
	
	# example image_array = 
	#   x --->
	# y [[122, 169, 17, .... , 117],
	# |  [....],
	# |  [....],
	# |  [....],
	# v  ...]
	
	plt.imshow(image_array, cmap='gray')
	plt.axis('off')
	plt.title("Random grayscale Image")
	plt.show()
	```
- **3 dimension / Rank 3 tensor :** (Example: a regular RGB image)
	```javascript
	// vanilla js
	let values = [
		[
			[114 191 105], [198 1 153], [235 84 213] ... [223 193 167]
		],
		[
			[162 33 214], [203 75 221], [ 95 105 80] ... [115 62 254]
		],
		......
	]
	
	// tensorflow.js
	let tensor = tf.tensor3d([
		[
			[114 191 105], [198 1 153], [235 84 213] ... [223 193 167]
		],
		[
			[162 33 214], [203 75 221], [ 95 105 80] ... [115 62 254]
		],
		......
	]);
	```
	Data Layout for RGB image, the tensor stores pixel data as:
	- `image_tensor[h][w][0]` → Red value of the pixel at height `h` and width `w`.
	- `image_tensor[h][w][1]` → Green value of the pixel at height `h` and width `w`.
	- `image_tensor[h][w][2]` → Blue value of the pixel at height `h` and width `w`.
	```python
	# import numpy as np
	import matplotlib.pyplot as plt
	import tensorflow as tf
	  
	# grayscale_image_tensor = tf.zeros((256, 256), dtype=tf.uint8);
	# image_array = grayscale_image_tensor.numpy()
	
	image_tensor = tf.random.uniform((256, 256, 3), minval=0, maxval=1)
	image_array = image_tensor.numpy()
	gray_array = tf.image.rgb_to_grayscale(image_array)
	
	plt.figure(figsize=(8,8))
	
	plt.subplot(1,2,1)
	plt.imshow(gray_array, cmap='gray')
	plt.axis('off')
	plt.title("Random grayscale Image")
	
	plt.subplot(1,2,2)
	plt.imshow(image_array)
	plt.axis('off')
	plt.title("Random rgb Image")
	plt.show()
	```
	Load an image and convert it to grayscale: 
	```python
	import tensorflow as tf
	import matplotlib.pyplot as plt
	
	image = tf.io.read_file('./test.png')
	image = tf.image.decode_image(image, channels=3) # convert image to RGB
	gray_image = tf.image.rgb_to_grayscale(image)
	
	plt.figure(figsize=(12, 6))
	
	# Original image
	plt.subplot(1, 2, 1)
	plt.imshow(image.numpy())
	plt.title("Original Image")
	plt.axis('off')
	
	# Grayscale image
	plt.subplot(1, 2, 2)
	plt.imshow(gray_image.numpy(), cmap='gray')
	plt.title("Grayscale Image")
	plt.axis('off')
	
	plt.tight_layout()
	plt.show()
	```
- **4 dimension / Rank 4 tensor :** (Example: a video where we have series of rank 3 tensor RGB images and 4th dimension is time)
- **5 dimension / Rank 5 tensor :** (Example: a batch of videos, Minecraft)
	In 3d games like Minecraft data is called voxel (volume element in a 3D space), where each voxel, so each voxel can have composition of `((r,g,b), x, y, z, time)` as a rank 5 tensor
- **6 dimension / Rank 6 tensor :** (Example: a batch of voxel, like a batch a voxel with animations(6th dimension))
### Attributes of a tensor :
- Data Types (DType, `dtype`)
	- `int8` === char stores 0-255
	- `int16` === short stores 0-2^16 - 1
	- `int32` === int stores 0-2^32 - 1
	- `int64` === int stores 0-2^64 - 1
- Shape
	Number of elements in each of dimension / axis
	Example: A Rank 3 with [4,5,8] will have 
		1st dimension will have 4 values in array
		2nd dimension will have 5 values in array 
		3rd dimension will have 8 values in array
- Rank / Axis
	Rank is simply the number of axis / dimension in an tensor
- Size
	its the total number of elements an tensor can hold
	Example: a rank 3 tensor of [4,5,8] can hold 4 * 5 * 8 = 160 elements

