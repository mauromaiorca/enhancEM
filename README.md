![enhancEM](docs/logo2_small.png)

enhancEM aims improve image contrast for cryoEM and cryoET data. It works with 2D and 3D images.

Additional features include FoM enhancement and FSC analysis from half maps. 

It is also possible to create fourier mask for the analysis providing the .tlt file.

 

# Download & Compile

Get the source code:
```
git clone https://github.com/mauromaiorca/enhancEM.git
```
This will local copy the file in your machine for your use.

Code can be compiled using [CMake]( https://cmake.org/install/](https://cmake.org/install/ ), with the commands:
```
cmake . 
make
cmake .
make
sudo make install
```
Note: enhancEM uses FFTW. If FFTW is not detected during installation, cmake tries to download and compile it. So, you need to have an active internet connection while compiling enhancEM.

# Pre-Built executables
 Pre-built executables are also available.
 
 MacOS Catalina: [https://github.com/mauromaiorca/enhancEM/raw/main/pre-built/enhancEM_mac]( https://github.com/mauromaiorca/enhancEM/raw/main/pre-built/enhancEM_mac )

 Linux:[https://github.com/mauromaiorca/enhancEM/raw/main/pre-built/enhancEM_linux]( https://github.com/mauromaiorca/enhancEM/raw/main/pre-built/enhancEM_linux )

# Usage

To produce usage information type:

```
enhancEM --help
```

the output is:

```
Usage: ./bin_mac/enhancEM  InputFile.mrc  [Options]
     Options: 
              --process sigmaSmoothPre[=1.0] sigmaSmoothPost[=0.3] sizePartition_list=[5:7:10]  [outputInternediateImageFile.mrc] numberOfHistogramThresholds[=1] HistogramBins[=40]
                     (sigma is in pixel)
              --halfmaps halfMap1.mrc halfMap2.mrc 
              --blur sigmaBlur outBlurredImage.mrc

          output parameters:
              --o outputApplyFilename.mrc 
              --replace outReplaceAmplitudeFilename.mrc (fully replace amplitudes to the input file)
              --msa  outInfoFile.msa (output info: mean shell amplitudes(msa), FSC, etc
              --apply inputInfoFile.msa outputProcessedFile.mrc

          Mask options: (mask should be 1 with data to retain, 0 data to ignore) 
              --fourierMask maskFile.mrc
              --tltMask data.tlt [tltMaskImageFileName.mrc]


          More options:
              --angpix pixelSpacing    (set the pixel size in A, default --angpix 1)
              --verbose
              --help
```

# Examples of usage

### Processing a 3D volume image

```
enhancEM virionB.mrc --o virionB_proc.mrc  --process 2.5 0.5
```

Parameters:

```
virionB.mrc input 3D     volumetric image in mrc format
--o virionB_proc.mrc     output 3D volumetric image in mrc format
--process 2.5 0.5.       process image, PreProcessing sigma Blur=2.5 pixels, Post-Processing sigma blur=0.5 pixel
```

Input/Output of the processing:

![processImage](docs/process_volume_example_small.png)

### Compute FSC

Given a full map "full_rec.mrc" and the two half maps "full_recH1.mrc" "full_recH2.mrc", Fourier Shell Correlation (FSC) and other Fourier shell measures, including CRef and "mean shell amplitude" (MSA), can be computed using the command

```
enhancEM_mac full_rec.mrc --halfmaps full_recH1.mrc full_recH2.mrc --msa fsc.csv
```

the output file will be the "fsc.csv", that can be opened in any csv processors (e.g. Excel) and plotted/analysed.
