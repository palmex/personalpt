# personalpt

PersonalPT is a lightweight, one-shot Repetitive Action Counting (RAC) model that easily generalizes to unseen exercises and is capable of running on a smartphone. When tested against state-of-the-art RAC models GB-DTW and TransRAC on three different datasets (Repcount, Fit3D, Variations), PersonalPT convincingly outperforms the SoTA models on the Fit3D and Variations dataset and achieves the best off-by-one (OBO) error score on the Repcount dataset. More details can be found in the performance section.

<details>
  <summary><b>model architecture</b></summary>

  PersonalPT is a gaussian mixture model that classifies each frame of a video as belonging to one of two states: Active State (State A), or Resting State (State R). The model assumes that in a video, the transition sequence State R --> State A --> State R represents one repetition. Each frame of the video is first skeletonized using Mediapipe v10.7, and then joint angles are calculated from the skeleton. During training, each frame of the video is labeled as either State A or State R. Using these labels, the model learns to associate certain skeleton data/joint angles with the states. When deployed, the model skeletonizes the input video, generates a predicted state for each frame of the video, and then counts the number of State R --> State A --> State R transitions to output a repetition count.

  ![image](https://github.com/user-attachments/assets/5ca8bb8e-c8a4-4f07-aa3b-2138e629e72b)

  
</details>

## quick start
download anaconda: https://www.anaconda.com/download/

run:
```
conda env create -f environment.yaml
conda activate seg_env
```
once all project dependencies are installed, if you want to train the model on a specific exercise, run:
```
python seg_mp.py --config PATH_TO_EXERCISE_CONFIG_FILE
```
for example, if you want to train the model on the deadlift exercise, run:
```
python seg_mp.py --config ./config_files/config_segmentation_deadlift.yaml
```

if you want to train the model on all exercises, run the bash script:
```
chmod +x run_all_seg.sh
./run_all_seg.sh
```

to run inference with the model, modify the prediction_js.py file to fit your own needs, then run it

## performance

PersonalPT was tested against Gesture-Based Dynamic Time Warping (GB-DTW) and TransRAC, 2 state-of-the-art RAC models on three different datasets (Repcount, Fit3D, Variations). Repcount includes "in-the-wild" videos gathered from public online sources, Fit3D includes videos created "in-the-lab" in a highly controlled environment (frame rate and subject positioning are consistent across videos), and Variations includes recordings created by volunteers following along with a demo video. We define off-by-one (OBO) error as the percentage of test videos where the model's outputted repetition count is within 1 of the ground truth repetition count. Similarly, we define mean absolute error (MAE) as the average prediction error of the model across all test videos. The performance of PersonalPT relative to the two SoTA models are shown in the following table:

<div align="center">
  <img src="https://media.discordapp.net/attachments/782820837816401951/1265790514637308015/image.png?ex=66a2cac7&is=66a17947&hm=f2cc3b446eae03a0bee8a030b213da5e5759915f92f519201af9edbaa5167887&=&format=webp&quality=lossless" width="350px">
</div>
