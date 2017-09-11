---
layout: project
title: Vehicle Detector
repository: https://github.com/guiklink/CarND-Vehicle-Detection
date:
image: https://github.com/guiklink/portfolio/blob/gh-pages/public/images/VehicleDetection/logo.png?raw=true
---

<article></article><br/>

# Vehicle Detection Project {#index-ignore}

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

<iframe src="https://vimeo.com/233159995" width="1137" height="639" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/233159995">Detecting Vehicles</a> from <a href="https://vimeo.com/user43396191">Guilherme Klink</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

[//]: # (Image References)
[image1]: ./output_images/car_not_car.png
[image2]: ./output_images/hog.png
[image3]: ./output_images/sliding_windows.png
[image4]: ./output_images/sliding_windows_HSV.png
[image5]: ./output_images/sliding_windows_imp.png
[image6]: ./output_images/heat_map.png


## Contents
* [Project Video](https://vimeo.com/233159995)
* [Train_Model](Train_Model.ipynb): notebook used for training a SVC.
* [Video_Pipeline](Video_Pipeline.ipynb): notebook used to produce the output video.
* [Trials](Trials.ipynb): notebook used to test different parameters for training.
* [auxiliary.py](auxiliary.py): contains all the functions used in the project.
* [model.p](model.p): a pickle file containing the trained model for HSV images and the parameters used.
* [model_YCrCb.p](model_YCrCb.p): a pickle file containing the trained model for YCrCb and the parameters used.

### 1. Histogram of Oriented Gradients (HOG)

To train a [Support Vector Classification (SVC)]() I used a large set of `vehicle` and `non-vehicle` images. I have attached an example bellow of `vehicle (left)` and `non-vehicle (right)` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

The code bellow found in lines 41 through 58 of the file called [auxiliary.py](auxiliary.py), is responsible for extracting the [HOG](https://en.wikipedia.org/wiki/Histogram_of_oriented_gradients) features from the image.  
```python
def get_hog_features(img, orient, pix_per_cell, cell_per_block, 
                        vis=False, feature_vec=True):
    # Call with two outputs if vis==True
    if vis == True:
        features, hog_image = hog(img, orientations=orient, 
                                  pixels_per_cell=(pix_per_cell, pix_per_cell),
                                  cells_per_block=(cell_per_block, cell_per_block), 
                                  transform_sqrt=True, 
                                  visualise=vis, feature_vector=feature_vec)
        return features, hog_image
    # Otherwise call with one output
    else:      
        features = hog(img, orientations=orient, 
                       pixels_per_cell=(pix_per_cell, pix_per_cell),
                       cells_per_block=(cell_per_block, cell_per_block), 
                       transform_sqrt=True, 
                       visualise=vis, feature_vector=feature_vec)
        return features
```
Here is an example using the `HSV` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image2]


### 2. Color Space and HOG Parameters

I tried various combinations of parameters and...

### 3. Training the SVC

I trained a linear SVM (or in this case SVC) in [Train_Model](Train_Model.ipynb) and saved it as a pickle in [model.p](model.p). 

The pickle contains the model and all the parameters used for creating the training features. This parameters also need to be used to transform chunks of images being analyze in an input feature to be classified by the model. 

```python
{'acc': 0.99270000000000003,
 'cell_per_block': 2,
 'hist_bins': 16,
 'model': LinearSVC(C=1.0, class_weight=None, dual=True, fit_intercept=True,
      intercept_scaling=1, loss='squared_hinge', max_iter=1000,
      multi_class='ovr', penalty='l2', random_state=None, tol=0.0001,
      verbose=0),
 'orient': 9,
 'pix_per_cell': 8,
 'rand_state': 75,
 'scaler': StandardScaler(copy=True, with_mean=True, with_std=True),
 'spatial_size': (16, 16)}
```

### 4. Sliding Window Search

Now that I have a model that is able to classify boxes of images, the next step of my approach was to segment each frame of the video into windows and feed them into my classifier. The code for that can be found in [auxiliary.py](auxiliary.py) and seem bellow:

```python
def find_cars(img, ystart, ystop, scale, svc, X_scaler, orient, pix_per_cell, cell_per_block, spatial_size, hist_bins, plot=False):
    
    draw_img = np.copy(img)
    # img = img.astype(np.float32)/255
    
    img_tosearch = img[ystart:ystop,:,:]
    ctrans_tosearch = convert_color(img_tosearch, conv='HSV')
    # ctrans_tosearch = convert_color(img_tosearch, conv='RGB2YCrCb')
    if scale != 1:
        imshape = ctrans_tosearch.shape
        ctrans_tosearch = cv2.resize(ctrans_tosearch, (np.int(imshape[1]/scale), np.int(imshape[0]/scale)))
        
    ch1 = ctrans_tosearch[:,:,0]
    ch2 = ctrans_tosearch[:,:,1]
    ch3 = ctrans_tosearch[:,:,2]

    # Define blocks and steps as above
    nxblocks = (ch1.shape[1] // pix_per_cell) - cell_per_block + 1
    nyblocks = (ch1.shape[0] // pix_per_cell) - cell_per_block + 1 
    nfeat_per_block = orient*cell_per_block**2
    
    # 64 was the orginal sampling rate, with 8 cells and 8 pix per cell
    window = 64
    nblocks_per_window = (window // pix_per_cell) - cell_per_block + 1
    cells_per_step = 2  # Instead of overlap, define how many cells to step
    nxsteps = (nxblocks - nblocks_per_window) // cells_per_step
    nysteps = (nyblocks - nblocks_per_window) // cells_per_step
    
    # Compute individual channel HOG features for the entire image
    hog1 = get_hog_features(ch1, orient, pix_per_cell, cell_per_block, feature_vec=False)
    hog2 = get_hog_features(ch2, orient, pix_per_cell, cell_per_block, feature_vec=False)
    hog3 = get_hog_features(ch3, orient, pix_per_cell, cell_per_block, feature_vec=False)
    
    window_list = []

    for xb in range(nxsteps):
        for yb in range(nysteps):
            ypos = yb*cells_per_step
            xpos = xb*cells_per_step
            # Extract HOG for this patch
            hog_feat1 = hog1[ypos:ypos+nblocks_per_window, xpos:xpos+nblocks_per_window].ravel() 
            hog_feat2 = hog2[ypos:ypos+nblocks_per_window, xpos:xpos+nblocks_per_window].ravel() 
            hog_feat3 = hog3[ypos:ypos+nblocks_per_window, xpos:xpos+nblocks_per_window].ravel() 
            hog_features = np.hstack((hog_feat1, hog_feat2, hog_feat3))

            xleft = xpos*pix_per_cell
            ytop = ypos*pix_per_cell

            # Extract the image patch
            subimg = cv2.resize(ctrans_tosearch[ytop:ytop+window, xleft:xleft+window], (64,64))
          
            # Get color features
            spatial_features = bin_spatial(subimg, size=spatial_size)
            hist_features = color_hist(subimg, nbins=hist_bins)

            # Scale features and make a prediction
            test_features = X_scaler.transform(np.hstack((spatial_features, hist_features, hog_features)).reshape(1, -1))    
            #test_features = X_scaler.transform(np.hstack((shape_feat, hist_feat)).reshape(1, -1))    
            test_prediction = svc.predict(test_features)
            
            if test_prediction == 1:
                xbox_left = np.int(xleft*scale)
                ytop_draw = np.int(ytop*scale)
                win_draw = np.int(window*scale)
                window_list.append(((xbox_left, ytop_draw+ystart),(xbox_left+win_draw,ytop_draw+win_draw+ystart)))
                if plot:
                    cv2.rectangle(draw_img,(xbox_left, ytop_draw+ystart),(xbox_left+win_draw,ytop_draw+win_draw+ystart),(0,0,255),6) 

    if plot:
        plt.imshow(draw_img)
                
    return window_list
```
#### Parameters: {#index-ignore}
* img: frame to be segmented.
* ystart, ystop: set a part of the image as an area of interest to search, after all cars won't be in the sky.
* scale: the size of each window.
* svc, X_scaler: this are the model trained and the scaler used to produce its features respectively.

#### Training Parameters: {#index-ignore}

* orient: define how many orientations for the gradient .
* pix_per_cell:  number of pixels per HOG cell.
* cell_per_block:  number of cells per HOG block.
* spatial_size, hist_bins: resize image and color histogram specs.
* plot: ```True``` or ```False``` to plot the output image.

**PS**: This parameters need to be set the same way they were for training the classifier.


![alt text][image5]

#### HSV vs. YCrCb

Through experimentation, I ultimately searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here you can see the two best performing color schemes ```HSV (Left)``` and ```YCrCb (right)``` :

![alt text][image4] ![alt text][image3]

---

### Video Implementation

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heat map from a series of frames of video around the image with detection showed before. Using the heat map strategy, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here the resulting bounding boxes are drawn onto the last frame in a 10 frames series: {#index-ignore}
![alt text][image6]

---

### Improvements
A clear aspect of my pipeline that could be improved is the fact that it sometimes detect cars coming in the opposite side of the road. One way to solve this could ignore the left side of the image, however that could create a blind spot if the car is all the way on the left of the lane. Another solution could be to reduce the scale of my windows when looking for a detection, that the drawback would be that the pipeline would not be able to detect cars far away on the correct road.

