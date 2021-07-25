# Integrating MJOLNIR into COS-POMDP codebase


Using the virtualenv of cos-pomdp, set up this code of MJLONIR.

1. Activate the cosp virtualenv

2. Install additional packages:

   ```
   pip install tensorboard
   pip install gym
   pip install einops
   pip install yacs
   pip install pytorch-lightning
   pip install segmentation-models-pytorch
   pip install -U scikit-learn
   ```

3. Run `python kb_agent.py --x_display 0 --env-name ThorObjs-v0`. Note `0` may need to be replaced with the output of `echo $DISPLAY`.
   (This runs the simulator. Not a demo)
   Go through the troubleshooting and make sure this works without issue.

4. Download data & model checkpoints
   ```
   bash interaction_exploration/tools/download_data.sh
   ```

5. Run the evaluation script:

   ```
   export UNETCKPT=`ls interaction_exploration/cv/intexp/run0/unet/*.ckpt`
python -m interaction_exploration.run \
    --config interaction_exploration/config/intexp.yaml \
    --mode eval \
    ENV.NUM_STEPS 1024 \
    NUM_PROCESSES 32 \
    EVAL.DATASET interaction_exploration/data/test_episodes_K_16.json \
    TORCH_GPU_ID 0 X_DISPLAY :0 \
    CHECKPOINT_FOLDER interaction_exploration/cv/intexp/run0/ \
    LOAD interaction_exploration/cv/intexp/run0/ckpt.24.pth \
    MODEL.BEACON_MODEL $UNETCKPT
   ```
   My computer 16GB RAM ran out of memory on this. But there was no error.


Check out [this note](https://github.com/zkytony/Notes/blob/master/Developer/ResearchProjects/setting_up_interaction-exploration.md) for troubleshooting.
