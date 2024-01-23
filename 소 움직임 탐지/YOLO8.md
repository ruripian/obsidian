
![[Pasted image 20240123130830.png]]





> 1. Replace the C3 module with the C2f module
> 
> 2. Replace the first 6x6 Conv with 3x3 Conv in the Backbone
> 
> 3. Delete two Convs (No.10 and No.14 in the YOLOv5 config)
> 
> 4. Replace the first 1x1 Conv with 3x3 Conv in the Bottleneck
> 
> 5. Use decoupled head and delete the objectness branch
