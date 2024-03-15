//required for build with openBLAS
//Change  /ggml/src/CMakeLists.txt  to  read

set(OPENBLAS_INCLUDE_SEARCH_PATHS
    /usr/include
    /usr/include/openblas
    /usr/include/openblas-base
    /usr/local/include
    /usr/local/include/openblas
    /usr/local/include/openblas-base
    /opt/OpenBLAS/include
    /data/data/com.termux/files/usr/include/openblas
    $ENV{OpenBLAS_HOME}
    $ENV{OpenBLAS_HOME}/include
    )


//with OpenBLAS
mkdir openblasbuild
cd openblasbuild
cmake .. -DGGML_OPENBLAS=ON
cmake --build . --config Release


//without OpenBLAS
mkdir purebuild
cd purebuild
cmake .. 
cmake --build . --config Release


termux-wake-lock

//main list of magic numbers 384 512 640 768 896

//sd 2.1 default txt2img
./purebuild/bin/sd -m "./models/v2-1_768-nonema-pruned.safetensors"  -o "/data/data/com.termux/files/home/storage/downloads/stableDiffusion/output_1.png" --steps 7 -b 1 -p "red car desert background"

//sd turbo txt2img
./purebuild/bin/sd -m "./models/sd_turbo.safetensors" -o "/data/data/com.termux/files/home/storage/downloads/stableDiffusion/output_1.png"  --steps 21 -b 1 -p "red car desert background"

//sd 1.5 with lcm-lora and tae-decoder txt2img
./purebuild/bin/sd -m "./models/v1-5-pruned-emaonly.safetensors"  -o "/data/data/com.termux/files/home/storage/downloads/stableDiffusion/output_1.png" --lora-model-dir "./models" --taesd "./models/taesd.safetensors" --cfg-scale 1 -H 384 -W 512 -p "red car desert background <lora:lcm:1>" --steps 3

//sd 1.5 with lcm-lora and tae-decoder img2img
./purebuild/bin/sd -M img2img -m "./models/v1-5-pruned-emaonly.safetensors"  -o "/data/data/com.termux/files/home/storage/downloads/stableDiffusion/output_1.png" --lora-model-dir "./models" --taesd "./models/taesd.safetensors" -i "/data/data/com.termux/files/home/storage/downloads/stableDiffusion/input/input.png" --cfg-scale 1 -H 384 -W 384 -p "silver aircraft blue sky ww2 clouds oil painting <lora:lcm:1>" --steps 3


termux-wake-unlock


