# Estimation Tutorial

## Problem Definition
One of the most important procedure of controlling drones to follow the pre-defined path is to obtain the pose (includes position and orientation) in the environment map. The procedure requires high-precision sensors and high-acuuracy algorithms. Here we use cameras with markers and correspondent computer vision algorhtims to estimate the pose.

## Localization by markers

## Transformation Model
Considering that the markers form a plane and the camera is taking images on the markers, we can establish a one-to one  mapping between points on this plane and points in the images. This is usually described by geometric tranformation. Here we will introduce a family of 2D geometric transformations starting from the simplest and working toward the most general.

### Euclidean transformations

## Pin-hole Model
According to Wikipedia, pinhole model is a simplified ideal camera model which describes the mathmatical relationship between the coordinates of a point in three-dimensional space and its projection onto the image plane. The model usually ignores geometric distortions or blurring of unfocused objects. Nonetheless, pin-hole model can be a practical generative first order approximation of the mapping from a 3D scene to a 2d image.

