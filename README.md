# libcaffe

## Implementation of Focal-Loss
The implementation of Focal-Loss Using Caffe;
### Usage
```
// Focal Loss layer
optional SoftmaxFocalLossParameter softmax_focal_loss_param = XXX; (XXX is determined by your own caffe)

message SoftmaxFocalLossParameter{
  optional float alpha = 1 [default = 0.25];
  optional float gamma = 2 [default = 2];
}

layer {
  name: "focal_loss"
  type: "SoftmaxWithFocalLoss"
  bottom: "ip2"
  bottom: "label"
  top: "focal_loss"
  softmax_focal_loss_param {
    alpha: 1 
    gamma: 1
  }
}
```
### Notes
Loss = -1/M * sum_t alpha * (1 - p_t) ^ gamma * log(p_t)
### Sigmoid Form
Here use `softmax` instead of `sigmoid` function.  
If you want see how to use `sigmoid` to implement `Focal Loss`, please see https://github.com/sciencefans/Focal-Loss to get more information.  


## Implementation of ROIAlign
The implementation of ROIAlign Using Caffe;
### Usage
```
// ROI Align layer
optional ROIPoolingParameter roi_pooling_param = XXX; (XXX is determined by your own caffe)

message ROIPoolingParameter {
// Pad, kernel size, and stride are all given as a single value for equal
// dimensions in height and width or as Y, X pairs.
optional uint32 pooled_h = 1 [default = 0]; // The pooled output height
optional uint32 pooled_w = 2 [default = 0]; // The pooled output width
// Multiplicative spatial scale factor to translate ROI coords from their
// input scale to the scale used when pooling
optional float spatial_scale = 3 [default = 1];
optional float pad_ratio = 4 [default = 0];
optional uint32 bi_type = 5 [default = 0];
optional bool is_multi_interpolate = 6 [default = True];
}

layer {
  bottom: "conv4_3"
  bottom: "rois"
  top: "roi_feaures"
  name: "roipooling"
  type: "ROIPooling"
  roi_pooling_param {
    spatial_scale: 0.0625
    pooled_h: 7
    pooled_w: 7
  }
}

layer {
  bottom: "conv4_3"
  bottom: "rois"
  top: "roi_feaures"
  name: "roialign"
  type: "ROIAlign"
  roi_pooling_param {
    spatial_scale: 0.0625
    pooled_h: 7
    pooled_w: 7
    bi_type: 0 #cancel the line if using default
    is_multi_interpolate: True #cancel the line if using default
  }
}
```