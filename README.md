# LeRobot GitHub Upload README

## Purpose

This document lists which files from the SO-101 block-into-cup project belong on GitHub, where they are stored on this PC, and which files are better published somewhere else. It also includes the training and testing commands used for this project.

## 1. Code files to upload to GitHub

The main code changes worth publishing for this project are:

- `C:\Users\Owner\lerobot\src\lerobot\scripts\lerobot_record.py`
- `C:\Users\Owner\lerobot\src\lerobot\configs\train.py`
- `C:\Users\Owner\lerobot\src\lerobot\scripts\lerobot_train.py`
- `C:\Users\Owner\lerobot\src\lerobot\utils\train_utils.py`
- `C:\Users\Owner\lerobot\tests\utils\test_train_utils.py`

These cover:

- local recording workflow changes
- training configuration updates
- checkpoint-to-Hugging-Face upload support
- tests for the checkpoint upload logic

## 2. Optional helper notes

Saved command notes on the desktop:

- `C:\Users\Owner\Desktop\Lerobot Record.txt`
- `C:\Users\Owner\Desktop\Lerobot Block in a Cup Training.txt`
- `C:\Users\Owner\Desktop\Lerobot Block in a Cup Test Command.txt`

These are optional. They can be uploaded if you want human-readable usage notes, but they are not required for the codebase.

## 3. Recorded dataset location

Your recorded dataset is here:

- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1`

Contents found in that dataset:

- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\data\chunk-000\file-000.parquet`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\meta\info.json`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\meta\stats.json`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\meta\tasks.parquet`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\meta\episodes\chunk-000\file-000.parquet`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\videos\observation.images.top\chunk-000\file-000.mp4`
- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1\videos\observation.images.wrist\chunk-000\file-000.mp4`

## 4. Training output location

Local training outputs are here:

- `C:\Users\Owner\act_runs\so101_block_into_cup_run1`

Saved checkpoints include:

- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\002000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\004000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\006000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\008000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\010000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\012000`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\last`

The best local checkpoint used for testing in this project was:

- `C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\012000\pretrained_model`

## 5. Hugging Face links

Model repo:

- [https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1](https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1)

Checkpoint folder on the Hub:

- [https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1/tree/main/checkpoints](https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1/tree/main/checkpoints)

Specific checkpoint used for testing:

- [https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1/tree/main/checkpoints/012000](https://huggingface.co/Histochemichael/so101-block-into-cup-act-v1/tree/main/checkpoints/012000)

Optional dataset link placeholder if the dataset is later published:

- `https://huggingface.co/datasets/<your-username>/so101-block-into-cup-v1`

## 6. What should go to GitHub

Recommended for GitHub:

- LeRobot code files
- Small docs and README files
- Command notes if you want reproducibility documentation

Usually not recommended for a normal GitHub code repo:

- Large `.mp4` files
- Large dataset `.parquet` files
- `model.safetensors`
- `optimizer_state.safetensors`
- entire `act_runs` checkpoint folders
- raw demonstration dataset folders

## 7. Recommended publishing strategy

Best split:

- GitHub repo: code, scripts, docs, README files
- Hugging Face Hub: trained models and uploaded checkpoints
- Dataset hosting: Hugging Face Hub or a separate dataset repo

That means:

- Put the LeRobot script and training support changes on GitHub
- Keep the dataset separate from the main code repo
- Keep large trained checkpoints on Hugging Face instead of in GitHub

## 8. System used and training requirements

System used for this project:

- OS: Windows 10
- GPU: NVIDIA GeForce GTX 1660 Ti
- GPU memory: 6 GB VRAM
- System memory: 64 GB RAM
- Python environment: `conda` environment named `lerobot`
- Training framework: LeRobot with CUDA-enabled PyTorch

Practical training requirements observed for this project:

- CUDA-enabled PyTorch must be installed and working in the `lerobot` environment
- At least 6 GB of VRAM was sufficient for the ACT training configuration used here
- Enough local disk space is needed for dataset storage, checkpoints, and temporary model downloads
- Internet access is needed if Hugging Face Hub checkpoint uploads are enabled
- Sleep should be disabled during long training runs to avoid interrupting training or uploads
- The training run described in this README used both wrist and 	op cameras
## 9. Training instructions

Environment setup:

```powershell
conda activate lerobot
hf auth login
```

Training command used for this project:

```powershell
lerobot-train --policy.type=act --dataset.repo_id=so101-block-into-cup-v1 --dataset.root=C:\Users\Owner\lerobot_data\so101-block-into-cup-v1 --output_dir=C:\Users\Owner\act_runs\so101_block_into_cup_run1 --policy.device=cuda --policy.push_to_hub=true --policy.repo_id=Histochemichael/so101-block-into-cup-act-v1 --policy.private=false --push_checkpoints_to_hub=true --checkpoint_hub_path=checkpoints --batch_size=1 --num_workers=2 --steps=30000 --log_freq=50 --save_freq=2000 --eval_freq=0 --policy.vision_backbone=resnet18 --policy.chunk_size=32 --policy.n_action_steps=16 --policy.dim_model=256 --policy.n_heads=4 --policy.dim_feedforward=1024 --policy.n_encoder_layers=2 --policy.n_vae_encoder_layers=2 --policy.latent_dim=16
```

Notes:

- This training run used both cameras from the dataset: `wrist` and `top`.
- Checkpoints were uploaded to Hugging Face Hub during training.
- A good checkpoint from this run was `012000`.

## 10. Real-robot test instructions

Before running the test:

- place the block and cup in the workspace before launching the command
- start the robot from the same or very similar pose used during recording
- make sure both cameras are connected and pointed at the same workspace arrangement used for training

Test command used for this project:

```powershell
cd C:\Users\Owner\lerobot
conda activate lerobot
lerobot-record --robot.type=so101_follower --robot.port=COM4 --robot.id=so101_follower --robot.max_relative_target=10 --robot.cameras="{wrist: {type: opencv, index_or_path: 0, width: 1280, height: 800, fps: 30}, top: {type: opencv, index_or_path: 1, width: 1280, height: 720, fps: 30}}" --dataset.repo_id=Histochemichael/eval_so101_block_into_cup_012000 --dataset.root=C:\Users\Owner\lerobot_eval --dataset.single_task="Put the block into the cup" --dataset.num_episodes=1 --dataset.episode_time_s=60 --dataset.reset_time_s=0 --dataset.push_to_hub=false --dataset.video=true --dataset.vcodec=h264 --policy.path=C:\Users\Owner\act_runs\so101_block_into_cup_run1\checkpoints\012000\pretrained_model --policy.device=cuda --display_data=true
```

Recorded robot/camera setup used for testing:

- `robot.type=so101_follower`
- `robot.port=COM4`
- `robot.id=so101_follower`
- `robot.max_relative_target=10`
- `wrist` camera: index `0`, `1280x800`, `30 fps`
- `top` camera: index `1`, `1280x720`, `30 fps`

## 11. Simple summary

Upload to GitHub:

- code changes
- tests
- small README and note files

Keep separate unless you intentionally want to publish the dataset or heavy artifacts:

- `C:\Users\Owner\lerobot_data\so101-block-into-cup-v1`
- `C:\Users\Owner\act_runs\so101_block_into_cup_run1`
- large model and optimizer checkpoint files
