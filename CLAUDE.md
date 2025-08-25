# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is "動手學深度學習" (Dive into Deep Learning, d2l-zh), the Chinese translation of the interactive deep learning textbook. It's a comprehensive educational resource that combines text, code, and executable notebooks for learning deep learning concepts.

## Framework Support

The book supports multiple deep learning frameworks:
- **PyTorch** (d2l/torch.py)
- **MXNet** (d2l/mxnet.py) 
- **TensorFlow** (d2l/tensorflow.py)
- **PaddlePaddle** (d2l/paddle.py)

Import the d2l library for a specific framework:
```python
from d2l import torch as d2l      # PyTorch backend
from d2l import mxnet as d2l      # MXNet backend  
from d2l import tensorflow as d2l  # TensorFlow backend
from d2l import paddle as d2l     # Paddle backend
```

## Development Commands

### Building the Book
- Build with specific framework: Use config.ini [build] section settings
- Build process converts markdown files to Jupyter notebooks (notebooks = *.md */*.md)
- Resources copied to build: img/, d2lzh/, d2l.bib, setup.py

### Package Installation
```bash
pip install -e .  # Install d2l package in development mode
```

### Testing
No explicit test commands found - uses AWS Batch for CI/CD:
- CPU builds: d2l-ci-cpu-builder
- GPU builds per framework: torch, tensorflow, mxnet, paddle
- Submit jobs via: `python ci/submit-job.py`

## Code Architecture

### Content Organization
- **chapter_**/: Book chapters as markdown files with executable code
- Each chapter has both original (*_origin.md) and translated versions
- **d2l/**: Python utility library with framework-specific implementations  
- **img/**: SVG/PNG images and diagrams
- **static/**: Web assets and build configurations

### Key Files
- **config.ini**: Build configuration, framework mapping, PDF/HTML settings
- **setup.py**: Package definition with core dependencies
- **STYLE_GUIDE.md**: Chinese translation and coding standards
- **TERMINOLOGY.md**: Technical term translations

### Markdown Structure
- Uses `:label:` for cross-references  
- `:begin_tab:` / `:end_tab:` for framework-specific code blocks
- Jupyter notebook outputs preserved in markdown
- Math equations in LaTeX format

### Code Patterns
- Framework-agnostic d2l utility functions
- Consistent variable naming (num_epochs, lr, net, etc.)
- Chinese comments in code
- Educational code style prioritizing readability

## Translation Workflow

Content flows: English original → Chinese translation
- **origin_repo**: d2l-ai/d2l-en 
- Translation managed through config.ini [translation] section
- Maintains parallel *_origin.md and translated .md files

## PyTorch 學習環境配置

### 專案結構
- **pytorch/** 資料夾：包含所有章節的 Jupyter Notebook 實作
- **spec_docs/** 資料夾：存放與此 repository 相關的文檔
- 主要專注於 PyTorch 框架，不使用其他深度學習框架

### Miniconda 虛擬環境設置
```bash
# 創建 PyTorch 學習環境 (Python 3.8+)
conda create -n d2l python=3.8
conda activate d2l

# 安裝 CUDA 支援的 PyTorch (適用於 RTX 3080)
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

# 安裝 d2l 套件
pip install d2l

# 安裝 Jupyter Notebook 相關套件
conda install jupyter ipython matplotlib pandas
```

### Cell Magic 標記用法
當需要修改 Jupyter Notebook 程式碼時，使用 `# %%` 標記將 .py 腳本分割成 cell：

```python
# %% [markdown]
# ## 章節標題

# %%
import torch
from d2l import torch as d2l

# %%
# 訓練模型的程式碼
net = torch.nn.Sequential(...)
```

### 學習工作流程
1. 使用 master 分支作為主要分支
2. 為學習創建新分支或使用 git worktree
3. pytorch/ 資料夾對應外層章節筆記
4. 所有修改都在虛擬環境中進行