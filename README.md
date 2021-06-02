# Goal-Aware Neural SAT Solver

This repository contains the official TensorFlow implementation of the following paper:
> **Goal-Aware Neural SAT Solver**
> 
> by  Emils Ozolins, Karlis Freivalds, Andis Draguns, Eliza Gaile, Ronalds Zakovskis, Sergejs Kozlovics 
> 
> [[arXiv]](https://github.com/LUMII-Syslab/QuerySAT)
> 
>  Abstract: _Modern neural networks obtain information about the problem and calculate the output solely from the input values. We argue that it is not always optimal, and the network's performance can be significantly improved by augmenting it with a query mechanism that allows the network to make several solution trials at run time and get feedback on the loss value on each trial. To demonstrate the capabilities of the query mechanism, we formulate an unsupervised (not dependant on labels) loss function for Boolean Satisfiability Problem (SAT) and theoretically show that it allows the network to extract rich information about the problem. We then propose a neural SAT solver with a query mechanism called QuerySAT and show that it outperforms the neural baseline on a wide range of SAT tasks and the classical baselines on SHA-1 preimage attack and 3-SAT task._

![Making Queries](assets/query_making.png)
The proposed query mechanism works by producing a query, evaluating it using an unsupervised loss function, and passing the resulting value back to the neural network for interpretation. It allows the model to obtain the structure and meaning of the solvable instance and information about the expected model output. The same unsupervised loss can be used for evaluating the query and for training.

## Requirements

To install requirements:

```sh
pip install -r requirements.txt
```

## Hardware requirements

* Ubuntu 20.04 or newer
* Nvidia T4 (16Gb) or better
* 16GB of RAM

## Training

To train the QuerySAT as in the paper, run this command:

```sh
python3 -u main.py --train --model <model_name> --task <task_name> --label <run_name>
```

It will generate data for the selected task and then train the selected model on it.
Valid model names are:
* `querysat` - For the QuerySAT model;
* `neurocore` - For the NeuroCore model;
* `neurocore_query` - For the NeuroCore model with added query mechanism.

Valid task names are:
* `ksat` - For the k-SAT task with 3 to 100 variables
* `3sat` - For the 3-SAT task with 5 to 100 variables
* `kcolor` - For the k-Color task for graphs with 4 to 40 vertices
* `clique` - For the 3-Clique task for graphs with 4 to 40 vertices
* `sha2019` - For the SHA-1 preimage attack from the [SAT Race 2019](http://sat-race-2019.ciirc.cvut.cz/) with 2-20 message bits

If you want to modify `config.py` or the appropriate model file in `models\`.


## Evaluation

To evaluate model on the test set of the task:

```sh
python3 -u main.py --evaluate --task <task_name> --model <model_name> --restore <checkpoint_directory>
```

By default formulas with same variable count as in training is used for evaluation. If you want to evalauted on larger formulas
please change `min_vars` and `max_vars` in generator code in appropriate `/data/*.py`.

To evalaute variable-wise generalization use:
```sh
python3 -u main.py --evaluate_variable_gen --task <task_name> --model <model_name> --restore <checkpoint_directory>
```

To evaluate step-wise generalization use:
```sh
python3 -u main.py --evaluate_round_gen --task <task_name> --model <model_name> --restore <checkpoint_directory>
```

## Results

Our model achieves the following performance on several SAT tasks:

| Model       | k-SAT  | 3-SAT  | 3-Clique | k-Coloring | SHA-1 |
| ----------- |--------| ------ |----------|----------- |-------|
| **_QuerySAT(proposed)_**      | **_99.05%_** | **_93.32%_** | **_94.74%_**   | **_98.32%_**     | **_46.57%_**|
| NeuroCore   | 50.82% | 57.63% | 1.04%    | 0.0%       | 0.24% | 

QuerySAT compared with NeuroCore:
![querysat_vs_classical](assets/querysat_vs_neurocore.png)

QuerySAT compared with classical solvers - GSAT and Glucose 4:
![querysat_vs_classical](assets/querysat_vs_classical.png)

## Contributing
For help or issues, please submit a GitHub issue.
For personal communications, please contact Emīls Ozoliņš (emils.ozolins@lumii.lv).
