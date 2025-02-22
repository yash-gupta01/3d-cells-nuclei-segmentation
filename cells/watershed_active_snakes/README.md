
## Dependencies 

- Image processing toolbox in MATLAB  

- `snakes3D` library from MATLAB file exchange:  Dirk-Jan Kroon (2022).  
  
- Snake: [Active Contour](https://www.mathworks.com/matlabcentral/fileexchange/28149-snake-active-contour), MATLAB Central File Exchange.

In the data, we provide the original stack of images with multiple cells [insert link].  We also process the dataset to extract a stack of single cells [insert link].  We use ImageJ software [1].

## Extracting the individual cell stacks 

1.  Import the image sequences *ch00* of the nucleus and *ch01* of the cell (stained for actin) into ImageJ.  Use File->Import->Image Sequence command and use the syntax (.*Series003_z[0-9][0-9]_ch00.*) to  import the image sequence 

2.  Make the maximum projection using the command Image->Stacks->Z Project of both the nucleus and  cell stacks 

3.  Use the polygon selection tool to manually draw out a cell on the maximum projection image of the cell. 

4.  Save the selection to ROI manager using the command Edit->Selection -> Add to Manager 5.  Transfer the cell boundary to the cell stack by selecting the stack and then selecting the boundary in the  ROI manager. 

5.  Duplicate just the part of the stack inside the cell boundary using the command Image->Duplicate 7.  Clear all the regions outside the cell in the duplicated stack using the command Edit->Clear outside 8.  Save the segmented cell stack into a separate folder appropriately named (Cell1, Cell2, etc.) using the  command File->Save as->Image Sequence 

6.  Repeat steps 5 to 8 for the nucleus stack and save the segmented nucleus image into the same folder 10.  Repeat steps 3 to 8 for all the cells in the stack.  Save each segmented cell and its nucleus stacks in  separate folders 

7.  Save all the manually drawn polygon cell boundaries of an image sequence using the save command in  the ROI manager 

8.  Import the next sequence and repeat steps 1 to 11. 


## Single cell segmentation

1.  `Main.m` It takes the location of the cropped cell from a multicell image stack or any other single cell stack from the dataset.

2.  `algo_snakes_3D.m` This function takes a 3D cell-matrix from the main file as input.  It does preprocess and extracts the initial approximate boundary that is an input to the snakes algorithm.  Parameters such as coefficients for ‘membrane energy’, ‘thin plate energy’, and ‘balloon force’, for the 3D surface for the snakes algorithm can be tuned here.

3.  `Snake3D.m` This function takes the initialized cell boundary and the parameters from algo_snakes_3D.m as input.  The function performs 3D segmentation using snakes algorithm to get the segmented 3D surface of the cells. 


## Watershed-Based Active Contours

If there is a stack with multiple cells, this method detects the cells, finds their approximate cell boundary, and an extra 3D stack of each cell to be individually segmented using the Active Snakes algorithm. 

1. `Marker_Watershed` This function detects the approximate cell location of each cell in the multicell image stacks.  It takes the location of the cell stack.  You can also provide a nucleus stack of the same cell for better segmentation results.  The output of the function is an approximate 2D location of each cell.
   
2.  `Active_Contours` This function takes the cell stack and the cell location as input to perform 3D segmentation. 

## References 

1. Schneider, C. A., Rasband, W. S., & Eliceiri, K. W. (2012).  NIH Image to ImageJ: 25 years of image analysis.  Nature Methods, 9(7), 671–675.  doi:10.1038/nmeth.2089
