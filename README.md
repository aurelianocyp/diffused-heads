[Demo](https://youtu.be/DSipIDj-5q0)


## Setup
Python 3.x environment
```
git clone https://github.com/aurelianocyp/diffused-heads
conda create -n diffused_heads python=3.8
conda activate diffused_heads
cd diffused-heads
pip install -r requirements.txt
conda install ffmpeg
```

## Sampling
Due to LRW license agreement, we are only able to provide a checkpoint of our model trained on CREMA.




1. Download and unpack [checkpoints](https://drive.google.com/file/d/1U90egQvzERHclTYPCjZadrEMyF7TAPa-/view?usp=drive_link) (our model and pretrained audio encoder).将两个checkpoint解压放进path/to中去
   

   
3. Specify paths and options in `config_crema.yaml` (check comments in the file).可以不更改直接运行
   
4. Run the script
```
python sample.py hydra.job.chdir=False
```
报错1：`RuntimeError: indices should be either on cpu or on the same device as the indexed tensor (cpu)`
在相应行之前添加上self.beta = self.beta.to(xt.device) 或者 self.log_beta_tilde_clipped = self.log_beta_tilde_clipped.to(xt.device)即可，注意需要更换相应self后的变量

The entire test set generated by our method can be downloaded from [here](https://drive.google.com/file/d/1zWSqtV7O4WGkgh6WB55b8Mdg2lXXUudH/view?usp=drive_link).这里面全是mp4视频，总大小40Mb

Download and unpack preprocessed CREMA [video](https://drive.google.com/file/d/1rM0FZLGiy-bJcxpv4CTlbUf0FuROubdk/view?usp=drive_link) and [audio](https://drive.google.com/file/d/1uS7Vi8EwarJFGQhsYHDMSkQmaNuiJIVW/view?usp=drive_link) files. 想看一看就可以下载
## Using your own data
### Audio
You can use audio recordings of your choosing freely. The only requirements are 16 kHz audio rate and a single audio channel. Please note our model is able to generate videos up to 9 seconds long depending on the audio.

### Identity frame
It is highly recommended to use a frame from the provided CREMA videos. This instance of the model was trained on clips with green background only. If you want to use your identity frame anyway, please follow this [repo](https://github.com/DinoMan/face-processor) for face alignment. Additionally, you may want to try segmenting the person and replacing background to green.

face alignment:
```
git clone https://github.com/DinoMan/face-processor
cd face-processor
conda create -n face_processor python=3.7
conda activate face_processor
pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu117
pip install face-alignment
pip install .
# pics_folder中放要aligment的图片
python main.py -i pics_folder -m mean_face -p -o out_folder --offset 0.11 0.335 0.155
```

## Training
The training code can be found in the branch [train](https://github.com/MStypulkowski/diffused-heads/tree/train). We aplogize for the delay.


