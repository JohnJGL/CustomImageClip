# CustomImageClip
Get image in the area of a custom bezier path efficiently

This framework get start with draw a bezier path follow the finger's track, then get the pixels contained by the bezier path

##Draw Bezier Path

The bezier path is created by CAShapLayer, whose path is a bezier path, with touch moves, addLine to new point.

This step is complecated when check if a bezier path is close.

##Modify pixels

Get the cgImage, and then get it's bitmap. The result is a array with element equal to image pixel count, each element is a Int32 * , each RGBA takes 8 bite.

The idea of process the image is quite simple: just enumerate the pixels, then call UIBezierPath's containsPoint: method. if is returns true, remains the pixel, if not, change the pixels to clear color(the Int32 value is 0)

##Effeiciency
If just do as said, there will comes a very terrible problem: time and memory cost, with a image take by iPhone 6s, 4224×3168, enumerate the pixels will cost about 12 second, and memory cost may takes 120MB.

###Resize The Image Display
The first step is to resize the image display on screen.

If the image size is much more lagger than screen size, we can scale the image consider the screen size. This obviously get the memory cost down.


###Resize The Image In BezierPath's Bounds
As you can think, we only get part of the image Pixels, maybe a very small part. So, crop the image displayed on screen with beizierPath's bounds. This step may be a little complecated when convert bezierPath coordinate to ImageSize coordinate

###Faster With Filter Array
From the last step, when we get the image within bezierPath, enumerate pixels. But as a result, most of the pixels are contained in the bezierPath, and not being modified.

To get faster, we can use array filter to get the point not contained by the bezier path, just change these pixels.

This step may be a little complecated when convert image pixel coordinate to bezierPath coordinate

With all we have done before, we can process a 4224×3168 pixels image within 0.5 second. How exciting!
