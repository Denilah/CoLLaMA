# LLM_Code： A Multilingual Instruction Dataset on Code and trained on large language models.
[![License](https://img.shields.io/badge/License-Apache_2.0-green.svg)](https://github.com/tatsu-lab/stanford_alpaca/blob/main/LICENSE) 
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/release/python-390/)

This is the repository for the `LLM_Code`project, which aims to build a multilingual instruction dataset on Code tasks. 

## Overview
As far as we know, at present, the dataset on instruction-tuning's code tasks is relatively messy, single-language,single-programming language, and the variety of tasks covered by the dataset is not wide enough. On the other hand, there are few open sourse datasets for instruction tuning in code tasks.

For this end, we propose this project, with the following advantages:
- 1. Multilingual Dataset: Include code samples from multiple programming languages(Java, Python, C, C#, Go, PHP, JavaScript, Ruby) to create a multilingual dataset. At the same time, this dataset also contains code examples in Chinese and English languages. This allows the model to learn instructions in different programming language contexts, making it more versatile.
- 2. Task diversity: Expand the dataset to cover a wide range of code trasks, including code summarization, code generation, code search.. This ensures that the instructions can handle different types of code tasks. And include a variety of code tasks with varying complexities and requirements. In addition, involve tasks of different levels, such as beginner, intermediate, and advanced, to cover a broad spectrum of programming skills and knowledge.
- 3. Multi-programming paradigms: Include code examples that cover different programming paradigms such as procedural, object-oriented, functional, or event-driven programming. This will provide a wider range of code tasks for the instruction-tuning model to learn from and generate instructions for.
- 4. Real-world code examples:  Include code snippets or excerpts from real-world projects to provide more realistic and practical code tasks. This helps the instruction-tuning model generate instructions that are applicable to real-world scenarios.
- 5. Quality assurance: Ensure the dataset has accurate and high-quality instructions for each code task.

The repository contains the following:
- The `MID_Dataset` used for fine-tuning the model
- The code for fine-tuning the model
- Model weight
- The code for evaluation

## Dataset release
[`data/MID_all_data.json`]() contains xx instruction-following dataused for fine-tuning the MID Code alpace model.
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

* **[code generation](data/code_generation)** : According to the natural languages input by the user, the corresponding code is generated.
* **[code summarization](data/code_summarization/)**: It aims to generate concise and readable summaries or description of source code. It involves automatically generating human-readable explations or summaries of code snippets, functions, or entire programs.
* **[code search](data/code_search/)**

    * **[code-to-code](data/code_search/code_to_code/)**

        * **[clone detection]()**: Given a piece of code, find another piece of code that is semantically related to it.
        * **[defect detection]()**: Given a source code, the task is to clarify what the specific defect of the code is. This include common errors such as null pointer, dereferences, array out of bounds, memory leaks, etc.
        * **[cloze test]()**: That involves completing or filling in the missing parts of a code snippet.
        * **[code repair]()**: It aims to automatically fix bugs in the code.
        * **[code translation]()**: Code translation refers to the process of converting source code from one programming language to another. It involves transforming the syntax, structure, and semantics of the original code while preserving its functionality and behavior.
    
    * **[query-to-code](data/code_search/query_to_code/)**: Given a natural language, the task is to search source code that matches the natural languag.

A brief summary of MID_dataset is given below:

<div style="text-align: center;">
<table border= "1" width= "600" align="center">
     <tr bgcolor="#D3D3D3">
        <td colspan=3>Task</td>  
        <td>Dataset name</td>  
        <td>Num</td>  
        <td>Programming Lang</td>
     </tr>
     <tr>
        <td colspan=3>Code summarization</td>  
        <td>CodeSearchNet</td>  
        <td>120k</td>  
        <td>Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
       <td colspan=3>Code generation</td>  
        <td>CodeSearchNet</td>  
        <td>120k</td>  
        <td>Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td rowspan=6>Code Search</td>  
        <td rowspan=5>code-to-code</td>  
        <td>Clone Detection</td>  
        <td>BigCloneBench</td>
        <td>20K</td>
        <td>Java</td>
     </tr>
     <tr>
        <td>Defect Detection</td>  
        <td>Devign</td>  
        <td>12460</td>  
        <td>C</td>
     </tr>
     <tr>
        <td>Cloze Test</td>  
        <td>CT-all</td>  
        <td>20K</td>  
        <td>Go,Java,JavaScript,PHP,Python,Ruby</td>
     </tr>
     <tr>
        <td>Code Repair</td>  
        <td>Bug2Fix</td>  
        <td>20K</td>  
        <td>Java</td>
     </tr>
     <tr>
        <td>Code Translation</td>  
        <td>CodeTrans</td>  
        <td>11749</td>  
        <td>Java,C#</td>
     </tr>
     <tr>
        <td colspan=2>query-to-code</td>  
        <td>CodePro</td>  
        <td> 　 </td>  
        <td>Python,SQL</td>
     </tr>
</table>
</div>

We mainly obtained datasets from [CodeSearchNet](https://github.com/github/CodeSearchNet) and [CodeXGLUE](https://github.com/microsoft/CodeXGLUE), processed them to obtain the aforementioned datasets, and concentrated them into one [dataset](data/MID_all_data.json).

## Finetuning

## Inference

## Citation
