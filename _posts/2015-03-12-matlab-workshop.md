---
title: "MATLAB Workshop"
categories:
  - Essays
tags:
  - essay
  - space
---

My department recently helped organize a workshop on image processing using Matlab's Image Processing and Image Acquisition toolbox. Around 50 students paid 400 INR to attend this two day workshop. Our instructor for these two days was Mr. Saurabh Bhardwaj from TechieNest.

I found the overall experience from these two days to be a lot underwhelming considering a lot of reasons. Before I go into the reasons and comment on the quality of it, I will encourage you to go through the notes to get an idea of teaching standards maintained at lower-tier engineering colleges in the country. My notes were typed during the workshop as-is.

---

Electronics Engineering has three major domains:

1. Embedded Robotics
2. VLSI uses two major languages, VHDL and Verilog.
3. Matlab

This workshop will focus on Matlab. It is a program that every engineer can use for a wide variety of tasks it can automate.

## Workshop introduction
We will be learning image processing in the next two days. Before we get start, an introduction of MATLAB.

* Matlab stands for Matrix Laboratory.
* Matlab has a lot of toolboxes that can be used by every engineering discipline.
* No single software can replace MATLAB right now. The power of Matlab comes from the fact that it is a consolidated tools for problems encountered by engineers.
* The usefulness of Matlab comes from the fact that computers are really useful with doing menial tasks. Say, we have a linear equation with 100 variables. It can be computed using hand but matlab makes the task much easier.
* Matlab can replace 50 lines of .NET code on average.
* Only software in market which claims 100% accuracy. Say, we want to launch a missile(where'd we launch it? Pakistan. Obviously) in that case, we will require a 100% accuracy.
* Matlab can do simulation. It provides simulation for all engineering disciplines.
* It is an interpreter. Biggest tool for all branches to work on for accuracy.

## Image Processing
* What is image processing?
* Major applications of image processing, security, traffic, satellite imagery, and sixth sense.
* EXE is one way to make things more interactive and avoid repetition of code everytime. Executables make use of command prompt or GUI.

## Making a bit
* How is a bit stored?
* Latch/Flip Flop -> NAND Gates -> CMOS and different gates
* VLSI fits all these components onto an IC.

## Installing Matlab
* Using a crack file. Instructions to activate the pirated copy of Matlab distributed to everyone in the seminar hall.

### Matlab - Main Window
* Command window is where we enter commands. Workspace, Current Directory and Command History.
* Some Matlab commands:
  * `clc` -- clear command prompt
  * `clear x` -- clears a variable x from the workspace
  * `exit` -- closes matlab when the previous instruction is finished executing.
  * `disp(x)` -- shows the contents of variable x or string.
* Don't save stuff in system drive(C:), it is where system boots from and saving it there will execute your program at boot.
* ans is a default object for all calculations.
* To delete a command, double click the command in command history.
* To suppress execution of a command, use a semicolon.
* numeral + string adds number to the ASCII value of the character. This can be used to implement Caesar Cipher.

## Writing our first script
* First challenge, make a calculator.
* To run an M-File, save it as X.m in your current directory. Type X in command window to run that file. Or, you can click the play button on the editor. Or, F5.

## Second Session
* for loop
* date and clock objects.
* Using fix(clock)
* making a stopwatch using pause()

##Image Processing
* Pixels is the smallest unit of an image.
* Resolution is the size of image.
* Use imagesc() to view matrix as images made pixel by pixel.
* What are static and dynamic images?
* Images taken from the webcam are dynamic images.

## Processing Static Images
* reading images using imshow()
* Display using imshow()
* imtool() can also display it along with some standard image processing operations.
* Exploring 'inspect pixel values'
* RGB is 3 layer image made from red, green and blue layers which are transparent. Values go from 0 to 255 on each layer.
* Greyscale use 1 value for each pixel. This value can be from 0 to 8 byte.
* Black and White further uses only 0 or 1 for each pixel.
* rgb2gray() and im2bw() and inter-conversions.
* differences in two images of equal resolution with imabsdiff()

The first day ended at 3:30 because a voice vote to extend the workshop failed.

## Introduction to Image Acquisition Toolbox
* Made rotate.m
* Pranav Mistry's Sixth Sense TED Talk
* Recognizing a specific color from the webcam stream

~~~
set a color as vector [r b g]
use a counter
iterate through all the pixels
check for color vector
~~~

* Similar method can be used with a range of color values maximum and minimum to identify different hues of the same color in dynamic images.
* Using this trigger to open a file or a website etc.
* Made count_color.m
* Made two_color.m which does different actions depending upon the color shown to it.

## Graphical User Interface
* Session starts after lunch at 12 noon.
* A GUI application that is a primitive camera application. It has 4 buttons that can
	1. Preview webcame live stream
	2. Click a snapshot
	3. Show image
	4. Convert the image to grayscale
* Explored MATLAB's Graphical User Interface Integrated Development Environment(GUIDE)
* The workshop concluded with a small quiz. 6 students were awarded with cash prizes.

## Project Ideas
* Gesture to speech conversion for the physically disabled.
* Make a video of image being rotated.
* Anti-theft system/Face detection
* Gesture controlled computer