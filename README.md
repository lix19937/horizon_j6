# horizon_j6


##  easy hb benchmark 

+ load image of docker
```bash
docker load -i docker_open_explorer_ubuntu_22_j6_gpu_v3.0.31.tar.gz   
```

显示如下 Loaded image: openexplorer/ai_toolchain_ubuntu_22_j6_gpu:v3.0.31   


+ install  
```bash   
docker run --gpus all -it -v /home/gwm/workspace/J6_start_3031/:/open_explorer \
  --shm-size 128G   openexplorer/ai_toolchain_ubuntu_22_j6_gpu:v3.0.31
```

+ check    
```bash  
hb_compile --march nash-m  --model $spec_onnx
```

+ fast perf   
```bash  
hb_compile --fast-perf  --march nash-m  --model $spec_onnx
```

+ config  
```bash 
hb_compile --config ./refnet_nuplan_mini_250116.yaml
```

+ fps   
```bash  
cd .fast_perf/      

open the *.html to get the fps and latency 
```

+ profile  on board   
```bash      
/userdata/.horizon/hrt_model_exec perf   --profile_path ./   \
   --thread_num 1 --frame_count 1000   --model_file ./float_sparse_lss_featuremap.hbm  


export LD_LIBRARY_PATH=/userdata/.horizon/lib:$LD_LIBRARY_PATH
```

+ 可视化   
```bash    
hb_model_info xxx.bc -v
hb_model_info xxx.hbm -v
```


+ demo  板端验证  
```bash
hrt_model_exec perf --model_file resnet50_224x224_nv12.hbm  \
--thread_num 1 --frame_count 1000 --input_stride="50176,224,1,1;25088,224,2,1" 
```

j6 默认相机输出为nv12（推荐的一个前提）， 因此即使onnx 模型以bgr-chw 方式进行格式输入的，如果在推理时期将输入改成nv12,而在hbm中增加一个n12_to_bgr-chw 的处理,利用硬件加速格式转换, 这种方式是没有引入误差的；
如果相机输出经过某些原因变成了bgr， 此时则不应该在推理阶段再使用nv12了，如果还使用nv12，则会引入转换误差以及耗时增加  


ptq 精度不及预期     

sudo mount -o nolock -t nfs 10.246.101.232:/share/data /home/gwm/workspaces/shared

export fp32 onnx     
https://developer.horizon.auto/blog/10174

forum    
https://developer.horizon.auto/forum  

learn abc      
https://www.cnblogs.com/horizondeveloper/p/18402297


--------------   

by lix   2025-01-20  



