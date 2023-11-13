# wenet_ov2023.1
run wenet by openvino 2023.1

## step1: download wenet code
```
$git clone https://github.com/wenet-e2e/wenet.git
```
## step2: modify OpenVINO version to OpenVINO2023.1
```
$cd wenet/runtime/core/cmake
```
replace openvino.cmake.

## step3: modify code to support the configuration of device name
```
$cd wenet/runtime/openvino/ov/
```
replace ov_asr_model.h and ov_asr_model.cc

```
$cd wenet/runtime/core/decoder/
```
replace params.h

```
$cd wenet/runtime/openvino/
```
replace CMakeLists.txt


## step4: install 
 For Ubuntu and Debian:
 
sudo apt install libtbb-dev libpugixml-dev

 For CentOS:
 
sudo yum install tbb-devel pugixml-devel

## step5: download Pre OpenVINO2023.1 version
```
$cd wenet/runtime/openvino/

$mkdir build && cd build

$cmake -DOPENVINO=ON -DTORCH=OFF -DWEBSOCKET=OFF -DGRPC=OFF ..

$make --jobs=$(nproc --all)
```
## step6： run demo
```
export GLOG_logtostderr=1

export GLOG_v=2

wav_path=your_test_wav_path

openvino_dir=your_model_dir

units=units.txt  # Change it to your model units path

./build/bin/decoder_main --chunk_size 16 \
    --wav_path $wav_path \
    --openvino_dir $openvino_dir \
    --openvino_device CPU \
    --unit_path $units 2>&1 | tee log.txt

python wenet/bin/export_onnx_cpu.py --config /exp/aishell_u2pp_conformer_exp/train.yaml --checkpoint /exp/aishell_u2pp_conformer_exp/final.pt --output_dir /exp/aishell_u2pp_conformer_exp/onnx/ --chunk_size 16 --num_decoding_left_chunks -1
```
