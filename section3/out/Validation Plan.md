# Validation Plan

## What is the intended use of the product?
The intended use of the product is to help doctors quantify the hippocampal volume based on deep learning techniques

## How was the training data collected?
We are using the "Hippocampus" dataset from the Medical Decathlon competition.  This dataset is stored as a collection of NIFTI files, with one file per volume, and one file per corresponding segmentation mask.  The original images here are T2 MRI scans of the full brain.  As noted, in this dataset we are using cropped volumes where only the region around the hippocampus has been cut out.  This makes the size of our dataset quite a bit smaller, our machine learning problem a bit simpler and allows us to have reasonable training times.  You should not think of it as "toy" problem, though.  Algorithms that crop rectangular regions of interest are quite common in medical imaging.  Segmentation is still hard.

## How did you label your training data?
All data has been labeled and verified by an expert human rater, and with the best effort to mimic the accuracy required for clinical use.

## How was the training performance of the algorithm measured and how is the real-world performance going to be estimated?
Metrics chosen to monitor for performance are Dice and Jaccard similarity to the ground truth, and the real-world performance is estimated by some experiment design and examined based on statistical inference.

## What data will the algorithm perform well in the real world and what data it might not perform well on?
In reality, if the hippocampal volume is identified with high accuracy, the algorithm will perform well; otherwise, the hippocampal volume is not easy to be segmented, then it might not perform well.
