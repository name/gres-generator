# GRES Generator

Generates SLURM GRES configurations for GPU clusters. Automatically maps GPUs to CPU cores.

## What it does

Looks at your GPUs and CPUs, then creates the config file SLURM needs to schedule GPU jobs properly.

## Install

```bash
curl -O https://raw.githubusercontent.com/name/gres-generator/main/gres-gen
chmod +x gres-gen
```

## Examples

**Basic usage:**

```bash
./gres-gen
```

```bash
Name=gpu        File=/dev/nvidia0 CPUs=0-15
Name=gpu        File=/dev/nvidia1 CPUs=16-31
Name=gpu        File=/dev/nvidia2 CPUs=32-47
Name=gpu        File=/dev/nvidia3 CPUs=48-63
```

**Custom GPU name:**

```bash
./gres-gen --name rtx4090
```

```bash
Name=rtx4090      File=/dev/nvidia0 CPUs=0-15
Name=rtx4090      File=/dev/nvidia1 CPUs=16-31
```

**Add to SLURM config:**

```bash
./gres-gen > /etc/slurm/gres.conf
systemctl restart slurmctld
```

**Test GPU scheduling:**

```bash
srun --gres=gpu:1 nvidia-smi
srun --gres=gpu:2 python train_model.py
```

## Options

```bash
./gres-gen --help
```

- `--name DEVICE` - Change device name (default: gpu)
- `--header FILE` - Include header file
- `--autodetect OPT` - Add AutoDetect line

## Requirements

- Linux with NVIDIA GPUs
- `nvidia-smi` command available
- SLURM installed

## That's it

The tool handles CPU core distribution automatically. Each GPU gets roughly equal CPU cores.

**Problems?** Check that `nvidia-smi -L` shows your GPUs.
