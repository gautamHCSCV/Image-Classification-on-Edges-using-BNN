# Binarized-Neural-Network-on-Edges
Classification of images based on edges only. The images are converted to their gray-scale format followed by application of sobel filter to detect the edges. Models are applied on this new set of images with only edges to detect their class.

**Sample Images**


![image](https://user-images.githubusercontent.com/65457437/137621498-8d913a18-1ae5-4c8e-91e4-b75f1cbc7e24.png)


Usually, we can easily identify objects in images where only a line drawing is given. Lines correspond to edges or sudden changes in images. Intuitively, most semantic and shape information from image can be encoded in the edges. It is more complicated presentation than original images. So, it can be very useful to extract edges from image, to use it for the recognition. The ideal result, is the drawing of an artist. But artist uses object-level knowledge. And in image processing, we really can use object-level knowledge. Now, we'll consider only basic algorithm without machine learning. But I will mention how the machine learning algorithm can be constructed for edge detection. We all can see the edges as points of rapid change in image intensity. Such points can be identified by considering first derivative of image intensity. Edges will correspond to local extrema of derivative. On the slide, I'll give a one-dimensional example, from one row in image. Because image is a two-dimensional function, we need to calculate image gradients as a vector of partial derivative of image intensity function. So, image gradient is a vector, where first element is partial derivative of f by x, and second element is partial derivative of f by y. The gradient direction is given by a tan of partial derivative of f by y over partial derivative of f by x. The edge strength is given by the gradient magnitude. Because images are discrete, we could approximate partial derivative of optical image. This finite difference in discrete images. Finite difference can be easily written as a simple convolution. For example, this 2 by 1 filter canel and Vel is minus 1 and plus 1. Other approximation of derivative filters exists. I put here a Roberts separator, Prewett, Sobel and Sharr operators. These operators both smothens and estimate finite differences in images. They produce visually similar results. Here are the example of edge maps, computed by convolution with these filters. You can see the difference between filter's is mostly small. Consider a single row or column in the real image. Usually, the image contained noise. The plotting intensity as function of position. Finite different filters respond strongly to noise. Noise results in pixels that look very different from the neighborhoods. So, finite difference use a lot of false responses. Generally, the larger the noise, the stronger the response and the larger the number of strong responses. So, if image contain noise, then the real edge can disappear. Like it's demonstrated in this slide. The solution,is to apply Gaussian smooths first to reduce the noise. After smoothen, edge detection kernel can identify the edge in our image. Because convolution is associative operation, we can combine both smooth and edge differentiation kernels into one filter kernel. This save us one operation and produce the same result, as applying smoothen and differentiation operator successively. Here are visualization for derivative of Gaussian filter which can be used for gradient estimation in real images. I give visualization of two filters. One estimate gradients in x direction, and one estimate gradient in y direction. Smooth derivative removes noise but blurs edges in images. The larger the Gaussian filter, the stronger the smoothen. As a result, we find images in different scales and appliance smoothen with different filter kernels. In this example, if use small filter, we get a lot of small edges, small details. When we use a larger filter with the gradients only corresponding to large objects in the image. Gradient magnitude estimation is not a complete edge detector. Edges are usually six stripes or edges of large image gradients, as you can see in this example. Also they receive only if the joint set of points is large gradients, without connectivity information that link neighboring points into real edges. This problems, were addressed by well known canny edge detector. This is probably the most openly used edge detection algorithm despite the fact that this was proposed in 1986. It has two steps for gradient estimation. First, there is non-maximum suppression. During the step, the thin multi-pixel of wide reaches down to a single pixel width. Second, is linking edge pixels together to form continuous boundaries. Here is the example. We take an original image of Lena, and apply gradient estimation first. The obtain map of gradient norm. You can notice a lot of thicker ridges at the edges. Now, we apply non-maximum suppression. This can be done in several ways. The easiest, is just to lose the pixels that a local maxima in the square neighborhood. For example, 3 by 3 pixels. The more complicated way, is to identify the points in either pixel rows along the image gradient vector. At q, We have a maximum if, its value is larger than those of both p and r. To get values in p and r, we should interpolate from neighbour pixels to get these values. So, we found a local maximum along the image gradient vector. Edge linking is performed as following, assume the marked point is an edge point, then reconstruct the tangent to the edge curve, which is normal to the gradient in this point. And use it to predict the next point. This light is either r or s points. We select the point with largest gradient and link it to the current point q. When linking edge points, we use hysteresis. Hysteresis is just two thresholds. We use high threshold to start edge curves and low threshold to continue them. This allows us to trace weak edges, if they're connected to the strong edges. Weak edges themselves won't be detected. This hysteresis threshold. Here is example of thinning on non-maximum suppression applies to the edge map obtained for this particular image. Hysteresis allow us to achieve a balance between the amount of detective edges and noise. If only one threshold is used, we either miss thick edges which are extensions of strong edges or get a lot of noise from weak edges alone. By varying the sigma in gradient computation all the size of Gaussian smoothen or the size of derivative for Gaussian filter. It can detect edges on different scales, large sigma detects large scale edges. Small sigma detects fine features in images. On this example, you can see that by increasing the size of sigma from 1 to 2, removes vertical lines originated from stripes on the row. The change in image intensity is not the only source of edges in real images. As can be seen from the slight change in color or texture, also gives us visible edges in images. But such changes cannot be detected by image gradient or canny operator. Of course there are more advanced methods which can achieve this result. Mostly, they can see that edge detection as pixel classification problem and use machine learning techniques. We create a data set of ground truth labeling of edges and trade in which classifier with classified pixels to either edges or non-edges by considering different features, like texture, color, image, intensity. But for many computer vision tasks canny edge detector proves to be sufficient. For example it's often used as feature extractor and produce features which are later used for image recognition.
