# Monodepth2 - Reimplement the code 
Github: https://github.com/nianticlabs/monodepth2

## **Environment**
#### **Create and start the new conda enviroment** 
```
$ conda create -n 3d_monodepth2 python=3.6
$ conda activate 3d_monodepth2
```
#### **Load package**
```
$ conda install pytorch=0.4.1 torchvision=0.2.1 -c pytorch
$ pip install tensorboardX==1.4
$ conda install opencv=3.3.1
$ pip install scikit-image
$ pip install ipython
```


---

## **Load Data**

#### Prepare dataset
Download dataset and put into the monodepth2 file

```
$ cd monodepth2
$ wget -i splits/kitti_archives_to_download.txt -P kitti_data/
```
![image](https://hackmd.io/_uploads/rkbDfVAta.png)
#### **Unzip**
* All file unzip
```
$ cd kitti_data
$ unzip "*.zip"
$ cd ..
```
* Specific file unzip
###### unzip only image_02 &image_03

```
$ cd kitti_data
$ find . -type f -name '*.zip' -exec sh -c 'unzip "{}" "*/image_02/*" "*/image_03/*" -d "$(dirname "{}")"' \;
```

![image](https://hackmd.io/_uploads/HkcUSH0Y6.png)
* image_02 : Left camera image
* image_03 : Light camera image 
* image_02 & image_03 Same quantity


---

## **Train**
Paper defaults require that you have converted the **png image to jpeg** using this command, Alternatively you can **train from raw png files** by adding the **- -png** flag when training, at the cost of slower loading times.
```
$ python train.py --model_name mono+stereo_model --frame_ids 0 -1 1 --use_stereo --png
```
#### **Error**
TypeError: 'tuple' object is not callable

![Screenshot from 2024-01-25 10-26-26](https://hackmd.io/_uploads/BkYzx1lqT.png)

#### [**Solve the error**](https://github.com/search?q=repo%3Anianticlabs%2Fmonodepth2+TypeError%3A+%27tuple%27+object+is+not+callable&type=issues)
Into **mono_dataset.py** change the code
```
color_aug = transforms.ColorJitter(
                self.brightness, self.contrast, self.saturation, self.hue)
```
![圖片1](https://hackmd.io/_uploads/HyTofyx9a.png)

#### **Sucessful**
![Screenshot from 2024-01-25 21-48-35](https://hackmd.io/_uploads/ryvaUkgca.png)

---


## Pred depth image
```
$ python test_simple.py --image_path assets/1711.jpg --model_name weights_19
```
#### **Error**
![image](https://hackmd.io/_uploads/Sk23Gix5p.png)
#### **Solve the error**
If you use the default values, you can **find the trained weight files** in the **/tmp** folder.

1. Copy weight
> Copy weights to **home/cluster/monodepth2/models**

![image](https://hackmd.io/_uploads/ryOmoye9T.png)


2. Add the name into code
>Add the weight file name to **test_simple.py*
>
![image](https://hackmd.io/_uploads/rJV3xie5p.png)

#### **Sucessful**
![image](https://hackmd.io/_uploads/SkBsXse56.png)

###### input
![test_image](https://hackmd.io/_uploads/Syyq5px5a.jpg)


###### output (our training weight)

![test_image_disp](https://hackmd.io/_uploads/SJg_4seqT.jpg)

###### output (paper training weight)

![test_image_disp](https://hackmd.io/_uploads/S1nnBslcp.jpg)
