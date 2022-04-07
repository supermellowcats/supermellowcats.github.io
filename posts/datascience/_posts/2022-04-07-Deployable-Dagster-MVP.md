# A deployable Dagster MVP (in 120 lines of Python)

Dagster describes itself as ‘the data orchestration tool designed for productivity’. And it’s a great tool. It’s also one with a steep learning curve. While learning Dagster, I found myself hunting for a simple example of a deployable data pipeline. It didn’t exist, so I built it myself. Here’s the Github repo: https://github.com/bakerwho/dagster-mvp.

Hopefully this will save you a lot of trial-and-error learning by giving you an example that works, from start to finish.

The simple pipeline we will run today performs three steps or `op`s:

1. Select 1 of 2 sentences based on an input `key`
2. Apply upper- or lower-case ‘normalization’
3. Strips the sentence of all punctuation

We build the pipeline as a Dagster `graph` that chains these 3 `op`s. We then configure the `graph` into a runnable `job`, and run the `job`. We also build a `scheduler` that can run this job every minute.

## Key Dagster concepts

Dagster lets you build **data pipelines** and orchestrate their execution .

A **data pipeline** is a set of compute operations that gets data from a **source**, **transforms** it to increase its value, and stores the finished ‘**data product**’ somewhere where it will be useful.

- The **source** could be a database, a file, or even the output of another pipeline
- The **transformation** could be one or more of data cleaning, visualization, normalization, ML training/inference, etc.
- The **‘data product’** could be ML predictions, BI dashboards, (I can’t think of a third example rn)

It’s useful to think of a **data product** as the end result because **products** have **functions** and any data pipeline should have a very clear function.

Next, a key concept in Dagster is the DAG.

A **DAG (directed acyclic graph)** is a set of nodes (say, A, B, C) and directed edges (directed edge A→B does not imply directed edge B→A) between them with no cycles (edges A→B→C implies no edge C→A).

If you’re still curious, go to [Wikipedia](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

## Putting it together

The point is, **data pipelines** should absolutely always be designed as **DAGs.** Why? Because data ops should flow in one direction without forming cycles.

Dagster lets you design your data `pipeline` as a DAG (directed acyclic graph) of operations (called `op`s). Each `pipeline` can be configured into a runnable `job`.

Other tools that do something similar are Apache [Airflow](https://airflow.apache.org/), Spotify’s [Luigi](https://github.com/spotify/luigi), Lyft’s [Flyte](https://flyte.org/), and the proprietary software Prefect. If you know any of these (except Prefect, I don’t care about that one), please write a blog post teaching it.

## Run the code

Step 0: `pip install dagster dagit`

1. **Clone the repo and `cd` to it using the following Terminal commands:**

```bash
git clone https://github.com/bakerwho/dagster-mvp

cd dagster-mvp

ls
```

It should look something like this:

```bash
.
├── README.md
├── dagster-exec
│   ├── dagster.yaml
│   └── workspace.yaml
└── pipeline_1.py
```

`dagster-exec` is where we will store all execution information, logs, etc. Everything outside of that can be ML code. Note that `dagster-exec` can have any name and can be stored anywhere on the machine. The only requirement is that it should contain a `workspace.yaml`.

1. **Edit 2 crucial lines to point to the correct filepaths.**

**Step 2A:** `.example_envrc` is a file containing environment variables. Some of these, like the AWS and Snowflake credentials are for illustratory purposes. Dagster requires that you set `DAGSTER_HOME` as the full path to the `dagster-mvp/dagster-exec` directory

```bash
export DAGSTER_HOME="/path/to/dagster-mvp/dagster-exec"
```

Once you have done this, type in your Terminal:

```bash
source .example_envrc
```

Pro tip: You can use `direnv` to automatically set environment variables in a `.envrc` file that persist within the scope of the parent directory and all subdirectories.

**Step 2B:** `dagster-exec/workspace.yaml` contains the following line which should point to the correct `.py` file containing a repo name as an `attribute`.

```bash
load_from:
  - python_file:
      relative_path: "/path/to/dagster-mvp/pipeline_1.py"
      attribute: repo_1
```

1. **Go through the `dagster` constructs in `pipeline_1.py`**

There are a few useful ones:

- a fake `resource` called `connection`: it takes a string `credentials` and turns it to uppercase). In a real use-case, you could use a `resource` for a database connection.
- a few fake `op`s: `get_string()`  selects one of two strings, `normalize_string()` turns an input string into upper or lowercase, and `clean_string` strips an input string of punctuation.
- a fake `IOManager`: an IOManager is attached to an `op` output, and it runs between the end of an upstream op and the start of a downstream op. This is a slightly tricky concept, but basically Dagster always wants to persist op results to memory - it is concerned that you will lose track of them unless they are saved to disk. The default IOManager pickles the results, whatever they may be. Like civilized ML Engineers, we want to write our string outputs to `.txt`, so we build something called `CustomIOManager` .
- the resource definitions and configuration needed to turn the `clean_string_graph` into `clean_string_job` using `clean_string_graph.to_job(resource_defs, config)`
- a `schedule` with the appropriate CRON string to run  `clean_string_job` every minute
1. **Run the damn thing in Python**

```bash
from pipeline_1 import clean_string_job
clean_string_job.execute_in_process()
```

1. **Run the damn thing from the command line**

```bash
dagster job execute clean_string_job
```

If it doesn’t work, double check the env variable `DAGSTER_HOME`.

1. **Run the damn thing from the pretty UI**

Run `dagit` to spin up a pretty local orchestration server.

```bash
dagit
```

Go to `[localhost:3000](http://localhost:3000)` in your browser.

You should be able to see your pipelines and jobs. Click on `clean_string_job` and go to the launchpad. You should be able to run it from here.

1. **Play with the scheduler**

Dagit’s UI also has a Schedules page. Click on it and you’ll see settings that let you turn the scheduler on and off. The scheduler is currently set to run every minute, but you can play with the Cron string. Changing code between or during scheduled runs leads to interesting results - play with the orchestrator to see what happens!

That’s it. It just works. If you dig around, there are a lot of other features you can play with. For example, I am currently uselessly passing an empty dict `hyperparameters` to an op that doesn’t use it.

If you had a massively complicated set of ML pipelines in different repos, you develop them independently but still orchestrate them together. You could chain operations using Dagster, launching runs of certain pipelines based on dependencies, periodic `schedules` or even custom `sensor`s that respond to an event. You could leverage multiprocessing to distribute big computations, and wait to collect the results before starting the next step. You could set retry conditions and smart error handling. You could keep your dev process clean by writing simple tests using dummy `config`s for predictable edge-cases.

Hopefully, you can now see the big picture, and have a tangible way to get started. Anything you can do in Python, you can do in Dagster - but now you can orchestrate it reliably in production.