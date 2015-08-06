# xrayinspect
A batch extractor of DICOM image data as well as a simple viewer.

# Summary
xrayinspect is intended to help automate measurements on large sets of DICOM images - particularly projection radiographs. It is written in Python as a command-line utility and measurements and image attributes are written to standard output with redirection to a *.csv file in mind. The intention is to be able to read entries in image metadata, simultaneously select a region of interest in each image to get signal and noise data, and then output these values together into one spreadsheet. For example,

	xrayinspect -e PatientID ExposureIndex DeviationIndex -r --fixed-size *.dcm >> Relevant_Anatomy.csv
	
steps over each DICOM file and appends PatientID, ExposureIndex, DeviationIndex, and Signal and Noise for each selected ROI to the file called Relevant_Anatomy.csv.

It is also possible to generate histograms of a region of interest or simply view an image. A secondary purpose of xrayinspect is to be a simple DICOM image viewer, since there are few DICOM viewers for GNU/Linux.

Lastly, xrayinspect was written to fit specific research needs. Although it is a very general tool, it is not guaranteed to be a solution for everyone, but hopefully it will save someone some trouble.

# Dependencies
1. Pydicom
2. Numpy
3. Scipy
4. Matplotlib

# Usage

	Usage: xrayinspect [Options] [Files]

	Options:
		--help
			Display this message and exit.
			
		-a
			Display all available attributes for the image and their values. This is the best way to access the metadata.
			
		-e [Attributes]
			Write out the values of the specified attributes in comma separated format in addition to any ROI information.
			All attributes are case sensitive. For example,
			
				xrayinspect -e PatientName PatientID TargetExposureIndex DeviationIndex xray.dcm
				'Bunsen Honeydew', 12345678, 400, 3.14
		
		-E
			Equivalent to -e AccessionNumber KVP XRayTubeCurrent TargetExposureIndex ExposureIndex DeviationIndex
		
		-h
			Display a histogram of the specified ROI.
		
		-r *kwargs
			If no argument is given, graphically specify a rectangular ROI. If -e or -E is also used, mean pixel value and
			standard deviation will be appended to the output data.
			
			If the keyword 'all' is used, the entire image will be selected as the ROI.
			
		--fixed-size
			Use with -r to specify square ROIs of constant size.
		
		-m
			Write out the raw metadata. This is often messy and it is recommended to use -a instead or pipe to grep.
			
		-s
			Display the specified image(s).
			
