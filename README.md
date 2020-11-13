# Semantic Segmentation for Indian Driving Scenes - ICVGIP 2020

Code for working with the dataset used for the [Semantic Segmentation for Indian Driving Scenes](http://cvit.iiit.ac.in/scene-understanding-challenge-2018/). For details of getting the dataset and updates see:

- http://cvit.iiit.ac.in/scene-understanding-challenge-2018/ 
- http://cvit.iiit.ac.in/autonue2018/

**The code has been tested on python 3.6.4**

## Dataset Structure 

The structure is similar to the cityscapes dataset. That is:
```
gtFine/{split}/{drive_no}/{img_id}_gtFine_polygons.json for ground truths
leftImg8bit/{split}/{drive_no}/{img_id}_leftImg8bit.png for image frames
```
### Semantic Segmentation

Furthermore for training, label masks needs to be generated as described below resulting in the following files:
```
gtFine/{split}/{drive_no}/{img_id}_gtFine_labellevel3Ids.png
```

## Labels

See helpers/anue_labels.py

### Generate Label Masks for Semantic Segmentation(for training/evaluation)

For the semantic segmentation challenge, masks should be generated using IDTYPE of level3Id and used for training models (similar to trainId in cityscapes). This can be done by the command:
```bash
python preperation/createLabels.py --datadir $ANUE --id-type level3Id --num-workers $C
```

- ANUE is the path to the AutoNUE dataset
- IDTYPE can be id, csId, csTrainId, level3Id, level2Id, level1Id.
- C is the number of threads to run in parallel

The generated files:

- _gtFine_labelLevel3Ids.png will be used for semantic segmentation


## Viewer

First generate label masks as described above. To view the ground truths / prediction masks at different levels of heirarchy use:
```bash
python viewer/viewer.py ---datadir $ANUE
```

- ANUE has the folder path to the dataset or prediction masks with similar file/folder structure as dataset.

TODO: Make the color map more sensible.


## Evaluation of Semantic Segmentation

First generate labels masks with level3Ids as described before. Then
```bash
python evaluate/evaluate_mIoU.py --gts $GT  --preds $PRED  --num-workers $C
```

- GT is the folder path of ground truths containing <drive_no>/<img_no>_gtFine_labellevel3Ids.png 
- PRED is the folder paths of predictions with the same folder structure and file names.
- C is the number of threads to run in parallel


## Acknowledgement

Some of the code was adapted from the cityscapes code at: https://github.com/mcordts/cityscapesScripts/ 
Some of the code was adapted from https://github.com/rbgirshick/py-faster-rcnn
Some of the code was adapted from https://github.com/cocodataset/panopticapi

