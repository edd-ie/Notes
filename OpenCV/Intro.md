#computer-vision #opencv 
## Setup on intellij
Follow video guide -> [clip](https://www.youtube.com/watch?v=TsUhEuySano)

## Loading OpenCV 

```java
import org.opencv.core.Core;

public class AppName {
        public static void main(String[] args){
                System.loadLibrary(Core.NATIVE_LIBRARY_NAME); //init
        }
}
```

## Data storage
### **Mat** [!]
- A class which represents an **n-dimensional** dense numerical single-channel or multi-channel array. 
- It can be used to store:
	- real or complex-valued vectors and matrices, 
	- grayscale or colour images, 
	- voxel volumes, 
	- vector fields, 
	- point clouds, 
	- tensors, histograms. 
- For more details check out the OpenCV [page](http://docs.opencv.org/3.0.0/dc/d84/group__core__basic.html).
```java
import org.opencv.core.CvType;
import org.opencv.core.Mat;

//->Example code<-//
Mat mat = Mat.eye(3, 3, CvType.CV_8UC1); // I(3x3)
```

**Mat.eye** -> a identity matrix, we set the dimensions of it (3x3) and the type of its elements.