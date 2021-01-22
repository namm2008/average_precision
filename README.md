# average_precision
Average Precision Calculation Example

## Introduction
Mean Average Precision (mAP) is widely used in the image classification tasks for Machine Learning community. Mean Average Precision 50% IoU ratio (mAP50) and 75% IoU ratio (mAP75) are the most commonly used Evaluation metric among all. 

Definition of Average Precision: AP= ∫0->1 p(r)dr where p(r) is function of precision given recall. The physical definition is the area under the precision over recall curve. In different competition, like the COCO dataset and Pascal VOC dataset, there are some subtle different practices in the calculations. In our investigation, the following approach was used in estimating the mAP50 and mAP75 value. 

```
For each class of object:
i. Find the intercept over union (IoU) of the ground truth masks and the predicted masks. In each predicted mask, the maximum value IoU across every ground truth in the same images are used. 
ii. Find the True Positive (TP) and False Positive (FP),\
      if IoU higher than the threshold, 
           (where threshold for AP50 was 50% and AP75 was 75% correspondingly) the observation is treated as TP, 
      else treated as FP.
iii. Find the accumulated TP and FP columns.
iv. Find the accumulated precision and recall by the TP, FP False Negative (FN). 
       precision=  TP/(TP+FP)              recall=  TP/(TP+FN)
       where (TP+FN) equals to the total number of ground truth masks.
v. Extract the precision and recall columns for plotting precision over recall graph. 
vi. Apply interpolation function p_interp (r)=max┬(r ̃≥r)⁡〖(r ̃)〗 to flatten the graph.
vii. Finally, the AP of the class was calculated by summing the area under curve (AUC). 
End for

Get the mAP value by the following equation: \
   mAP=  (∑(q=1)->(q=Q)〖AP(q)〗)/Q
   where Q is the total number of class.
```

Here was some additional implementation detail:
1.	In step I, for each image, a for-loop was created for all the masks produced from the model. Each mask was then compared with all the ground truth with same class by IoU. The highest IoU was used as the final IoU of the particular mask. A list of all the masks in an image was created with 4 inputs say the image number, the class name, confidence scores, and maximum IoU. The list then concatenated to form the dataset for procedure ii to vii.
2.	For easy manipulation, Pandas DataFrame was implemented in procedure ii to vii. Pandas DataFrame is a tool renowned for the data wrangling. In procedure ii to vii, some calculation between columns and creating new columns, and plotting graphs were applied. 
3.	In procedure vi, the graph was flattened to ease the finding of AUC in the procedure vii. Fig 1. showed the visualization of the graph before and after applying the interpolation. 

![]()
*Fig. 4 The precision-recall curve of a Class showed the visualization from procedure vi*



