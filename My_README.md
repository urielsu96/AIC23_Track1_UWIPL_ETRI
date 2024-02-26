# Explanation
S001 se refiere a escena 1, que en este caso es la unica escena con informacion real
Todos los demas son escenas sinteticas simulando un ambiente 3D

data/test_det:
ID camara, Frame, ID Persona (?), Bbox [3 a 6], Probabilidad (confianza)
c001,0,1,1468,290,1885,922,0.9981431

Contiene para cada camara, cada frame de deteccion de personas, cada persona detectada y muy IMPORTANTE contiene la lista de todos 

data/test_emb:
Embeddings para cada escena en forma matricial en numpy

data/SCT:
Contiene la información de tracking por cada camara, sin hacer alguna integración.
Para cada camara en cada escena lo registra en un archivo de texto (ej. S003_c014.txt -> escena 3 camara 14) con la siguiente información
Frame, ID Persona, Bbox [2 a 5], Probabilidad, (?) [7 a 9]
0,1,1468.00,290.00,417.00,632.00,1.00,-1,-1,-1

data/hungarian_cluster:
Contiene parte del Multi-Camera Tracking, en particular lo referente al tracking con IDs globales.
Usa de referencia data/SCT para obtener 
Frame, ID Persona, Bbox [2 a 5], Probabilidad, (?) [7 a 9]
0,4,1160.86,611.51,97.6,296.36,0.97,-1,-1,-1


# Training
```sh
sudo apt install ffmpeg
pip install ffmpeg-python
bash scripts/0_prepare_mtmc_data.sh
```

# Inference
```sh
conda activate botsort_env
cd BoT-SORT
gdown 1P4mY0Yyd3PPTybgZkjMYhFri88nTmJX5 # ByteTrack YOLOX_x
python3 tools/aic_get_detection_S001.py --device=cpu -f yolox/exps/example/mot/yolox_x_mix_det.py -c bytetrack_x_mot17.pth.tar ..
```

# Embeddings
```sh
conda activate torchreid
cd deep-person-reid
cd checkpoints
gdown 1UxUI4NsE108UCvcy3O1Ufe73nIVPKCiu
gdown 1Sk-2SSwKAF8n1Z4p_Lm_pl0E6v2WlIBn
gdown 1YjJ1ZprCmaKG6MH2P9nScB9FL_Utf9t1
gdown 1vduhq5DpN2q1g4fYEZfPI17MJeh9qyrA
gdown 112EMUfBPYeYg70w-syK6V6Mx8-Qb9Q1M
cd ..
python3 torchreid/aic_extract_S001.py ..
```

# Tracking
```sh
cd BoT-SORT
conda activate botsort_env
python tools/run_tracking.py ..
```

You need CUDA for the following section
```sh
conda activate mmpose
cd mmpose
python demo/top_down_video_demo_with_track_file.py ../data/hungarian_cluster_S001/S001_c001.txt configs/body/2d_kpt_sview_rgb_img/topdown_heatmap/coco/hrnet_w48_coco_256x192.py https://download.openmmlab.com/mmpose/top_down/hrnet/hrnet_w48_coco_256x192-b9e0b3ab_20200708.pth --device=cpu --video-path ../data/test/S001/c001/video.mp4
```

Change run_sctra.py with feet eneabled as False
