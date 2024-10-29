# Monodepth2 in own data
You can read this paper to understand the technical details. However, I am applying this technology to medical data to obtain depth estimation .
> **Digging into Self-Supervised Monocular Depth Prediction**
>
> [Clément Godard](http://www0.cs.ucl.ac.uk/staff/C.Godard/), [Oisin Mac Aodha](http://vision.caltech.edu/~macaodha/), [Michael Firman](http://www.michaelfirman.co.uk) and [Gabriel J. Brostow](http://www0.cs.ucl.ac.uk/staff/g.brostow/)
>
> [ICCV 2019 (arXiv pdf)](https://arxiv.org/abs/1806.01260)
> 
## ⚙️ Setup
Create and start the new conda enviroment
```shell
conda create -n monodepth2 python=3.6
conda activate monodepth2
```
You can follow the Monodepth2 GitHub to set up the environment.
## 💾 Prepare training data
Change own data or use KITTI Data

### If you want to train with your data, you must convert the data into a .txt file for reading

### Downlad KITTI Data
```shell
wget -i splits/kitti_archives_to_download.txt -P kitti_data/
```

Specific file unzip --> unzip only image_02 & image_03
```shell
cd kitti_data
find . -type f -name '*.zip' -exec sh -c 'unzip "{}" "*/image_02/*" "*/image_03/*" -d "$(dirname "{}")"' \;
```
## 🔧 Solve problems encountered during training
#### TypeError: 'tuple' object is not callable
Enter **mono_dataset.py** and modify this section of the code.
```
color_aug = transforms.ColorJitter(
                self.brightness, self.contrast, self.saturation, self.hue)
```
