# Training_System
This is a tutorial of how tp train the MobileNet-SSD detection network. The implementation of MobileNet-SSD detection network used was taken from [here](https://github.com/chuanqi305/MobileNet-SSD).

### Run
1. Download the caffe source code from [BVLC](https://github.com/BVLC/caffe) or [here](https://github.com/weiliu89/caffe/tree/ssd) and compile.
2. Put the MobileNet-SSD directory in caffe/examples.
3. Run demo.py.

### Create lmdb for your own dataset
1. Prepare the caffe-ssd VOC training datasets.
2. Put MyDataSet directory in caffe/data.
3. Replace the content in MyDataSet/Images with the VOC training datasets Images (should be original images), replace  and content in MyDataSet/Labels with the VOC training datasets Labels (should be xml files). Each image in Images folder should have a unique label file in Labels folder with same name.
4. Modify MyDataSet/create_data.py file if necessary.
5. Create MyDataSet/ImageSets/Main.
6. Run MyDataSet/create_data.py, which will generate four txt files in ImageSets/Main.
7. Modify the paths and directories in `create_list.sh` and `create_data.sh` as specified in same file in comments (The scripts was taken from [here](https://github.com/chuanqi305/MobileNet-SSD)).
8. Modify `labelmap_voc.prototxt` according to your classes defined in the caffe-ssd VOC training datasets.
9. Run `create_list.sh` and `create_data.sh`.

### Train MobileNet-SSD network
1. Go to caffe/examples/MobileNet-SSD
2. Run `gen_model.sh` with one argument `n` where n is the number of your classes defined in MyDataSet/labelmap_voc.prototxt (e.g. `bash gen_model.sh 2` and there are 2 classed including background defined in MyDataSet/labelmap_voc.prototxt).
3. Create symlinks.
```
ln -s PATH_TO_YOUR_TRAIN_LMDB trainval_lmdb
ln -s PATH_TO_YOUR_TEST_LMDB test_lmdb
```
4. Modify `train.sh` if necessary (Currently the training mode is CUP-ONLY).
5. Run `train.sh` which will generate snapshots in caffe/examples/MobileNet-SSD/snapshot directory.
6. Run `merge_bn.py` to obtain the caffemodel and prototxt based on snapshots generated from the previous step. One example:
```
python merge_bn.py --model ./example/MobileNetSSD_deploy.prototxt --weights ./snapshot/mobilenet_iter_50000.caffemodel
```