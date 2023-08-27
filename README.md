# CoLLaMA: A Multilingual Instruction Dataset and Large Language Model for Code
[![License](https://img.shields.io/badge/License-Apache_2.0-green.svg)](https://github.com/tatsu-lab/stanford_alpaca/blob/main/LICENSE) 
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/release/python-390/)

\[ English | [中文](README_zh.md) \]

This is the repository for the `CoLLaMA` project, which aims to build a multilingual (Chinese and English) instruction tuning dataset and large language model for coding tasks. 

<p align="center" width="100%">
<img src="https://i.postimg.cc/J7Ds1tw6/CoLLaMA.jpg"  width="40%" height="20%">
</p>

## Overview
Current code instruction datasets, which are essential for instruction-tuning tasks, are often disorganized, monolingual, and single-programming language focused, while covering an insufficient variety of tasks. Open-source datasets for instruction tuning in coding tasks are also scarce.

For this end, we propose this project, with the following advantages:
- `Multilingual Dataset`: Our dataset incorporates code samples from a multitude of programming languages including Java, Python, C, C#, Go, PHP, JavaScript, and Ruby et.al. It also presents code instructions in both Chinese and English, enabling the model to learn in various programming language and spoken language contexts, and thereby enhancing its generalization ability.
- `Task diversity`: The dataset spans a broad range of coding tasks, such as code summarization, code generation, code search, and others. It incorporates tasks with varying complexities and requirements, from beginner to advanced levels. This comprehensive approach ensures our instructions can handle different types of coding tasks and covers a broad spectrum of programming skills and knowledge.
- `Multi-programming paradigms`: The project includes code examples from different programming paradigms, such as procedural, object-oriented, functional, and event-driven programming. This wide coverage provides the instruction-tuning model with a varied set of coding tasks to learn from and generate instructions for.
- `Real-world code examples`: The dataset incorporates code snippets or excerpts from actual projects or forums such as StackOverflow and Github, to present more realistic and practical coding tasks. This aids the instruction-tuning model in generating instructions applicable to real-world scenarios.
- `Quality assurance`: We are committed to providing an accurate and high-quality dataset for each coding task. For instance, the instruction dataset for code search, extracted from programming posts on Stackoverflow Q&A sites, is rigorously filtered and cleaned to ensure its usability in real Q&A applications.

The repository contains the following:
- The `MulCo` used for fine-tuning the model
- The code for fine-tuning the model
- Model weight
- The code for evaluation

## Dataset release
`data/MID_train_EN_data` and `data/MID_train_CN_data` contains around 88k instruction-following data used for fine-tuning the CoLLaMA model.
This file is a list of dictionaries, each dictionary contains the following fileds:
- `instruction`: describes the task that the model should perform. 
- `input`: optional code or context for the task. For example, if the instruction is 'Please summarize this PHP code.', the input is the PHP code.
- `output`: the answer to the instruction. 

All data in our collection is formatted into the same templates, where each sample is as follows:
```
[
{"instruction":  `string`,
"input":  `string`, # (may be empty)
"output": `string`}
]
```

Due to the different code tasks, we choose which filed to generate with  `gpt-3.5-turbo` or human. Unlike `self-struct` technology to generate data, most of the code in our data comes from the real world, whereas most instruction choices are generated by `gpt-3.5-turbo`. The detailed process of data processing is described in the next section.

## Dataset Collection & Processing
It includes 8 datasets for 8 diversited code tasks covering the following scenarios:

* **[code generation](data/code_generation)**: According to the natural languages input by the user, the corresponding code is generated.
* **[code summarization](data/code_summarization/)**: It aims to generate concise and readable summaries or description of source code. It involves automatically generating human-readable explations or summaries of code snippets, functions, or entire programs.
* **[code search](data/code_search/)**

    * **[code-to-code](data/code_search/code_to_code/)**

        * **[clone detection]()**: Given a piece of code, find another piece of code that is semantically related to it.
        * **[defect detection]()**: Given a source code, the task is to clarify what the specific defect of the code is. This include common errors such as null pointer, dereferences, array out of bounds, memory leaks, etc.
        * **[Code Completion(line level)]()**: Complete the unfinished line given previous context. 
        * **[code repair]()**: It aims to automatically fix bugs in the code.
        * **[code translation]()**: Code translation refers to the process of converting source code from one programming language to another. It involves transforming the syntax, structure, and semantics of the original code while preserving its functionality and behavior.
    
    * **[query-to-code](data/code_search/query_to_code/)**: Given a natural language query and mutiple code snippets, the task is to search source code that its function matches the natural languag query.

A brief summary of MulCo is given below:

<table border= "1" width= "600" align="center">
     <tr bgcolor="#D3D3D3">
        <td colspan=3 align="center">Task</td>  
        <td align="center">Source Dataset name</td>  
        <td align="center">Num</td>  
        <td align="center">Lang</td>  
        <td align="center">Programming Lang</td>
     </tr>
     <tr>
        <td colspan=3 rowspan=2 align="center">Code summarization</td>  
        <td align="center">CodeSearchNet</td>  
        <td align="center">10k</td>  
        <td align="center">EN</td>  
        <td align="center">Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td align="center">CodeSearchNet</td>
        <td align="center">10K</td>
        <td align="center">CN</td>
        <td align="center">Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
       <td colspan=3 rowspan=3 align="center">Code generation</td>  
        <td align="center">CodeSearchNet</td>  
        <td align="center">10k</td>  
        <td align="center">EN</td>  
        <td align="center">Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td align="center">CodeGPT</td>
        <td align="center">20k</td>
        <td align="center">CN</td>
        <td align="center">C#,C,C++,Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr> 
        <td align="center">CodeSearchNet</td>  
        <td align="center">5k</td>  
        <td align="center">CN</td>  
        <td align="center">Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td rowspan=7 align="center">Code Search</td>  
        <td rowspan=5 align="center">code-to-code</td>  
        <td align="center">Clone Detection</td>  
        <td align="center">BigCloneBench</td>
        <td align="center">10k</td>
        <td align="center">EN</td>  
        <td align="center">Java</td>
     </tr>
     <tr>
        <td align="center">Defect Detection</td>  
        <td align="center">Devign</td>  
        <td align="center">5K</td> 
        <td align="center">EN</td>   
        <td align="center">C</td>
     </tr>
     <tr>
        <td align="center">Code Completion(line level)</td>  
        <td align="center">CodeSearchNet</td>  
        <td align="center">5K</td>  
        <td align="center">EN</td>  
        <td align="center">Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td align="center">Code Repair</td>  
        <td align="center">Bug2Fix</td>  
        <td align="center">5K</td>  
        <td align="center">EN</td>  
        <td align="center">Java</td>
     </tr>
     <tr>
        <td align="center">Code Translation</td>  
        <td align="center">CodeTrans</td>  
        <td align="center">5k</td>  
        <td align="center">EN</td>  
        <td align="center">Java,C#</td>
     </tr>
     <tr>
        <td colspan=2 rowspan=2 align="center">query-to-code</td>  
        <td align="center">CodePro</td>  
        <td align="center">10K</td>  
        <td align="center">EN</td>  
        <td align="center">Python,SQL</td>
     </tr>
     <tr>
        <td align="center">CodePro</td>
        <td align="center">5k</td>
        <td align="center">CN</td>
        <td align="center">Python,SQL</td>
     </tr>
</table>

We mainly obtained datasets from [CodeSearchNet](https://github.com/github/CodeSearchNet),[CodeXGLUE](https://github.com/microsoft/CodeXGLUE),  [codeGPT](https://github.com/zxx000728/CodeGPT) and [CodePro](https://github.com/hoogang/CodePro), processed them to obtain the aforementioned datasets, and concentrated them into one [dataset](data/MID_all_data.json).

## Finetuning
So far, considering the influence between different data tasks, we currently only use the Code generation、Code summarization、code completion、code query datasets for fine-tuning. At the same time, we added the [codealpaca](https://github.com/sahil280114/codealpaca) dataset.

The fine-tuning process is basically followed [Firefly](https://github.com/yangjianxin1/Firefly).

To reproduce a fine-tuned version of QWen, please follow the steps below.

In order to effectively finetune a `QWen-7b` model, we used QLora technology to train on an `A100 80GB` GPUs. Meanwhile, you need to adjust the training parameters according to your GPUs and dataset.

Before fine-tuning, first make sure to install all requirements using:

```bash
pip install -r requirements.txt
```

Below is the command to fine-tune QWen-7B using our dataset combined with QLoRA technology on an "A100 80G" GPU machine.

```bash
torchrun --nproc_per_node=1 --master_port='29502' train_qlora.py --train_args_file QWen-7b-sft-lora.json
```
The main fine-tuning parameters are as follows:

```bash
"train_file": "/data/MID_train_512_EN_52K.jsonl",
"num_train_epochs": 1,
"per_device_train_batch_size": 6,
"gradient_accumulation_steps": 2,
"learning_rate": 1e-5,
"max_seq_length": 512,
"logging_steps": 50,
"save_steps": 500,
"save_total_limit": 1,
"lr_scheduler_type": "constant_with_warmup",
"warmup_steps": 500,
"lora_rank": 64,
"lora_alpha": 16,
"lora_dropout": 0.05,
"gradient_checkpointing": true,
"optim": "paged_adamw_32bit",
"fp16": true,
"dataloader_num_workers": 0,
"save_strategy": "steps",
"weight_decay": 0,
"max_grad_norm": 0.3
```

You can replace `train_file` with your own dataset.

The above fine-tuning command only saves the weight and configuration file of the adapter, and needs to merge the weight of the adapter with the base model. Merge script see `merge_lora.py`


## Evaluation (TODO)

## Citation
<div>
<div align="center">
    <a target='_blank'>Gang Hu<sup>1</sup></span>&emsp;
    <a target='_blank'>Xi Wen<sup>1</sup></span>&emsp;
    <a target='_blank'>Xin Liu<sup>1</sup></a>&emsp;
    <a href='https://jimin.chancefocus.com/' target='_blank'>Jimin Huang<sup>2</sup></a>&emsp;  
    <a target='_blank'>Qianqian Xie*<sup>3</sup></a>&emsp;   
    
</div>
<div>
<div align="center">
    <sup>1</sup>School of Information Science & Engineering, Yunnan University&emsp;
  <sup>2</sup>ChanceFocus AMC&emsp;
   <sup>3</sup>School of Computer Science, Wuhan University&emsp;
</div>
   
```
@misc{Hu2023CoLLaMA,
      title={CoLLaMA: A Multilingual Instruction Dataset and Large Language Model for Code}, 
      author={Gang Hu and Xi Wen and Xin Liu and Jimin Huang and Qianqian Xie},
      year={2023},
}
```
</div>
</div>
