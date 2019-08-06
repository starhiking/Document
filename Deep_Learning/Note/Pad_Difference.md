# Pad difference mode between caffe and tensorflow.

Pad mode make an influence on some layers ,like **convolution** , **depthwise** , **pooling** and so on.

* ## Pad calculator mode in caffe
  ### caffe is implemented output size by specifically controlling the pad value.
      output_height = floor (( input_hight + 2 x pad_height - kernel_height ) / stride_height ) + 1
      output_width = floor (( input_hight + 2 x pad_width - kernel_width ) / stride_width ) + 1

* ## Pad calculator mode in tensorflow
  ### tensorflow is using padding type to control output size and pad value.
  1. ### Padding = Same 
    "Same" tried to pad evenly left and right , but if the amount of columns to be added is **odd**, it will add the extra column to the right.
      
      output_height = ceil( input_height / stride_height )
      need_pad_height = ( output_height - 1) * stride_height + kernel_height - input_height
      pad_top = floor( need_pad_height / 2 )
      pad_down = need_pad_height - pad_top

    2. ### Padding = Valid
    "VALID" only ever drops the **right**-most columns (or **bottom**-most rows)

      output_height = ceil ((input_height - kernel_height + 1 )/stride_height)
      pad = 0

* ## tensorflow VS caffe
|tensorflow|caffe|remark|
|-         | -   | -    |
|```output_height = ceil (( input_height - kernel_height + 1) / stride_height)```|```output_height = floor (( input_height - kernel_height + 2 x pad_height )/stride_height ) + 1 ```|calculator formula|
|Pad = "Same" , strides = 1|Pad = kernel / 2 , strides =1|only when **pad_top = pad_down** in tensorflow|
|Pad = "VALID"|Pad = 0|equivalent|
  
