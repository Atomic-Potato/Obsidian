Filtering technique is a process used to restore the original image by removing noise from the noisy image.

Causes of noise are:
- Sensor heat
- Poor transmission
- Memory failure

A noisy image can be modeled as:
$$\large \color{cyan} n(x,y)= o(x,y) + b(x,y)$$where
- `n:` noisy image
- `o:` original image
- `b:` noise
![[Pasted image 20220727223906.png | 400]]

# Noise Types
## Line drop
![[Pasted image 20220727223954.png | 300]]
## Salt and Pepper
![[Pasted image 20220727224021.png]]
## Gaussian
The histogram of the noisy image has the form of Gaussian distribution
![[Pasted image 20220727224055.png | 300]]
![[Pasted image 20220727224110.png | 100]]

--- 

# Spatial Filtering
Spatial filtering changes pixel’s intensity according to the intensities of its neighbors.

## Linear filtering: Average
We use this equation:
$$\large \color{cyan} F = K (*) N$$
Where:
- `F`: filtered image
- `K`: Kernel, mask or filter.
- `N`: Noisy image
- `(*)`: Convolution operator

A kernel is usually a 3\*3 matrix
![[Pasted image 20220727225711.png]]

So its like $\large \color{cyan} F(i,j)=sum(sum(K(u,v)*N(i+u,j+v)))$ 
The first sum adds each entry with its equivalent in position in the second row
Second sum adds all entries of the matrix

Each pixel in the output image contains the average value in a 3-by-3 neighborhood (kernel size) around the corresponding pixel in the input image.
![[Pasted image 20220727225939.png | 500]]

- **Implementation of `(*)`:**
	- the elementwise multiplication: `.*`
	- The function sum

*Example:*
```matlab
W=[1 2 3;4 5 6;7 8 9];
K=[1 1 1;1 1 1;1 1 1]/9;
R=sum(sum(W.*K))
```

**Average filter function:**
```matlab
function [im] = AvgFilter(G, windowSize)
	boundarySize = (windowSize-1)/2;
	
	im = zeros(2*boundarySize+size(G,1), 2*boundarySize+size(G,2)); %%an empty matrix with boundaries
	im(boundarySize+1:size(im,1)-boundarySize, boundarySize+1:size(im,2)-boundarySize) = G; %%fill the matrix with G except for the boundaries
	
	W = ones(windowSize, windowSize) / (windowSize*windowSize);

	for i=boundarySize+1 : size(im,1)-boundarySize
		for j=boundarySize+1 : size(im,2)-boundarySize
			currentWindow = A(i-boundarySize:i+boundarySize , j-boundarySize:j+boundarySize);
			im(i,j) = sum(sum(currentWindow.*W));
		end
	end

	im = uint8(im); % converting the matrix to int values
	im = im(boundarySize+1:size(im,1)-boundarySize, boundarySize+1:size(im,2)-boundarySize) %removing the crust aka the boundaries
end
```

## Non-linear Filtering: Median
Replace each pixel in the noisy image with the median of its neighbors.
- create the corresponding window `N(i-1:i+1, j-1:j+1)`
- sort values using `sort(sort(M,2));`
- get the middle using `median(median(M));`

Each pixel in the output image contains the median value in a 3-by-3 neighborhood (window size)around the corresponding pixel in the input image.
![[Pasted image 20220727230412.png | 500]]


**Median filter function:**
```matlab
function [im] = MedFilter(G, windowSize)
	boundarySize = (windowSize-1)/2;
	
	im = zeros(2*boundarySize+size(G,1), 2*boundarySize+size(G,2)); %%an empty matrix with boundaries
	im(boundarySize+1:size(im,1)-boundarySize, boundarySize+1:size(im,2)-boundarySize) = G; %%fill the matrix with G except for the boundaries
	
	for i=boundarySize+1 : size(im,1)-boundarySize
		for j=boundarySize+1 : size(im,2)-boundarySize
			currentWindow = A(i-boundarySize:i+boundarySize , j-boundarySize:j+boundarySize);
			currentWindow = sort(sort(M,2));
			im(i,j) = median(median(currentWindow));
		end
	end

	im = uint8(im); % converting the matrix to int values
	im = im(boundarySize+1:size(im,1)-boundarySize, boundarySize+1:size(im,2)-boundarySize) %removing the crust aka the boundaries
end
```

---

# Notes

- **`NOTE:`** notice how im doing the calculations over the original image and just changing the values in a copy of the image
- How to add boundaries
![[Pasted image 20220916214547.png | 700]]