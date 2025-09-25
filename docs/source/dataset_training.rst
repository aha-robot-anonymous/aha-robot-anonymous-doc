Build Dataset, Train Model & Inference
======================================

This guide will walk you through the complete process of collecting datasets, training models, and running inference on your AhaRobot system.

Collect a Dataset
-----------------

On your AhaRobot host machine, run the following to collect a dataset:

.. code-block:: bash

   # rm -rf /home/rosdev/.cache/huggingface/lerobot/YOUR_NAME/astra_grab_floor_toys_extended # if you want to start over
   # rm -rf /home/rosdev/.cache/huggingface/lerobot/YOUR_NAME/astra_grab_floor_toys_extended/images # if collection was interrupted
   pushd non_ros_src/lerobot
   python lerobot/scripts/control_robot.py \
     --robot.type=astra_joint \
     --control.type=record \
     --control.single_task="Grab the toys on the floor and put them on the tabletop." \
     --control.fps=30 \
     --control.repo_id=YOUR_NAME/astra_grab_floor_toys_extended \
     --control.tags='["astra"]' \
     --control.warmup_time_s=0 \
     --control.episode_time_s=-1 \
     --control.reset_time_s=-1 \
     --control.num_episodes=50 \
     --control.push_to_hub=true \
     --control.resume=false \
     --control.local_files_only=true \
     --control.run_compute_stats=true

Configuration Notes
~~~~~~~~~~~~~~~~~~~

- ``--control.num_episodes=50`` sets the number of demos to collect.
- ``--control.push_to_hub=false`` prevents uploading data to Hugging Face.
- ``--control.resume=true`` appends to an existing dataset.
- ``--control.run_compute_stats=false`` skips dataset statistics to speed up collection.

Inspect Dataset Contents
------------------------

After collection, you can use rerun to visualize the dataset:

.. code-block:: bash

   pushd non_ros_src/lerobot
   python lerobot/scripts/visualize_dataset.py --repo-id YOUR_NAME/astra_grab_floor_toys_extended --episode-index 51 --local-files-only 1

.. warning::
   If you see 3D/GPU errors when running rerun inside Docker, launch rerun outside Docker.

Train Your Model
----------------

Train on your collected dataset:

.. code-block:: bash

   python lerobot/scripts/train.py \
     --dataset.repo_id=YOUR_NAME/astra_grab_floor_toys_extended \
     --policy.type=act \
     --output_dir=outputs/train/act_astra_grab_floor_toys_extended \
     --job_name=act_astra_grab_floor_toys_extended \
     --device=cuda \
     --wandb.enable=true

Evaluate the Model
------------------

Run your trained model in the real world:

.. code-block:: bash

   pushd non_ros_src/lerobot
   # rm -rf /home/rosdev/.cache/huggingface/lerobot/YOUR_NAME/eval_act_astra_grab_floor_toys_extended # if restarting evaluation
   python lerobot/scripts/control_robot.py \
     --robot.type=astra_joint \
     --control.type=record \
     --control.fps=30 \
     --control.single_task="Grasp the toys on the floor and put them on the tabletop." \
     --control.repo_id=YOUR_NAME/eval_act_astra_grab_floor_toys_extended \
     --control.num_episodes=1 \
     --control.warmup_time_s=0 \
     --control.episode_time_s=-1 \
     --control.reset_time_s=-1 \
     --control.push_to_hub=false \
     --control.policy.path=outputs/train/act_astra_grab_floor_toys_extended/checkpoints/0040000/pretrained_model \
     --control.run_compute_stats=false \
     --control.display_cameras=false
