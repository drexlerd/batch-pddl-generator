# Batch PDDL Generator (BPG)

Specify PDDL generator parameters and their value ranges and let BPG generate
PDDL tasks for you.


## Installation

Create a virtual environment:

    python3 -m venv --prompt sig .venv
    source .venv/bin/activate
    pip install -U pip wheel
    pip install -r requirements.txt

Clone repo with PDDL generators:

    git clone git@github.com:AI-Planning/pddl-generators.git


## Usage

There are two ways in which this library can be used:

1. Generate the Cartesian product of instances over the given parameter values.

    For example, when you specify the following domain

        Domain(
            "tetris",
            "generator.py {rows} {block_type} {seed}",
            [
                get_int("rows", lower=4, upper=8, step_size=2),
                get_enum("block_type", ["1", "2", "3"], "1"),
            ],
        ),

    the command

        /generate-all-instances.py \
            --generators-dir <path/to/generators> \
            tetris /tmp/tasks

    will generate instances at /tmp/tasks for the following combination of
    rows and blocks:

        [(4, 1), (4, 2), (4, 3), (6, 1), (6, 2), (6, 3), (8, 1), (8, 2), (8, 3)]


2. Use SMAC to generate planning tasks that can be solved by a given planner
within given resource limits.

        ./generate-instances.py \
            --generators-dir <path/to/generators> \
            --planner-time-limit 60 \
            tetris <path/to/singularity-planner.img>
