---
layout: post
title: "The simplest deployable Dagster pipeline (in 120 lines of Python)"
categories: DataScience
comments: true
published: true
excerpt: Get Dagster running in 5 minutes ...
tags: code Python MLOps dagster
---

# Get Dagster running in 5 minutes

[Dagster](https://dagster.io/) describes itself as ‘the data orchestration tool designed for productivity’. And it’s a great tool. It’s also one with a steep learning curve. While learning Dagster, I found myself hunting for a simple example of a deployable data pipeline. One Minimum Viable Product that just *works*. I couldn't find one, so I built it myself.

Here’s the Github repo: [https://github.com/bakerwho/dagster-mvp](https://github.com/bakerwho/dagster-mvp). If you follow this 5-minute read, you will know what's good about Dagster and how to get a simple Dagster pipeline running.

Hopefully this will save you a lot of trial-and-error learning by giving you an example that works, from start to finish.

The embarrassingly simple pipeline we will run today performs three steps or `op`s:

1. Select 1 of 2 sentences based on an input `key`
2. Apply upper- or lower-case ‘normalization’
3. Strips the sentence of all punctuation

We build the pipeline as a Dagster `graph` that chains these 3 `op`s. We then configure the `graph` into a runnable `job`, and run the `job`. We also build a `scheduler` that can run this job every minute.

## Key Dagster concepts

Dagster lets you build **data pipelines** and orchestrate their execution.

A **data pipeline** is a set of compute operations that gets data from a **source**, **transforms** it to increase its value, and stores the finished ‘**data product**’ somewhere where it will be useful.

- The **source** could be a database, a file, or even the output of another pipeline
- The **transformation** could be one or more of data cleaning, visualization, normalization, ML training/inference, etc.
- The **‘data product’** could be ML predictions, BI dashboards, (I can’t think of a third example rn)

It’s useful to think of a **data product** as the end result because **products** have **functions** and any data pipeline should have a very clear function.

Next, a key concept in Dagster is the DAG.

A **DAG (directed acyclic graph)** is a set of nodes (say, A, B, C) and directed edges (directed edge A→B does not imply directed edge B→A) between them with no cycles (edges A→B→C implies no edge C→A).

If you want to know more, go to [Wikipedia](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

## Putting it together

The point is, **data pipelines** should absolutely always be designed as **DAGs.** Why? Because data ops should flow in one direction without forming cycles.

Dagster lets you design your data `graph` as a DAG of `op`s. You then configure a `graph` into a runnable `job`. You can run a `job`s with a direct call from Python/the CLI - you can also orchestrate it with complicated dependencies.

Other tools that do something similar to Dagster are Apache [Airflow](https://airflow.apache.org/), Spotify’s [Luigi](https://github.com/spotify/luigi), Lyft’s [Flyte](https://flyte.org/), and the proprietary software Prefect. If you've used any of these and prefer it to Dagster, let us know why (except Prefect, I don’t care about that one). Type a comment below!

## Run the Dagster job

**Step 0 and the most important step is to** `pip install dagster dagit`

1. **Clone the repo and `cd` to it using the following Terminal commands:**
    ```bash
    git clone https://github.com/bakerwho/dagster-mvp

    cd dagster-mvp

    ls
    ```

    It should look something like this:

    ```
    .
    ├── README.md
    ├── dagster-exec
    │   ├── dagster.yaml
    │   └── workspace.yaml
    └── pipeline_1.py
    ```

    `dagster-exec` is where we will store all execution information, logs, etc. Everything outside of that can be ML code. Note that `dagster-exec` can have any name and can be stored anywhere on the machine. The only requirement is that it should contain a `workspace.yaml`.

2. **Edit 2 crucial lines to point to the correct filepaths.**

    **Step 2A:** `.example_envrc` is a file containing the ENVIRONMENT variables you need to set to run your Dagster project. Some of these, like the AWS and Snowflake credentials are for illustratory purposes. However, Dagster does prefer that you set `DAGSTER_HOME` variable with the path to your local `dagster-mvp/dagster-exec` directory

    ```bash
    export DAGSTER_HOME="/path/to/dagster-mvp/dagster-exec"
    ```

    Once you have done this, activate these environment variables by running:

    ```bash
    source .example_envrc
    ```

    Pro tip: You can use `direnv` (website [here](https://direnv.net/)) to automatically set environment variables in a `.envrc` file that persist within the scope of the parent directory and all subdirectories.

    **Step 2B:** `dagster-exec/workspace.yaml` contains the following line. Edit it to point to the correct `.py` file and repo within it as an `attribute`. In this case, it is `repo_1` defined in `pipeline_1.py`:

    ```bash
    load_from:
      - python_file:
          relative_path: "/path/to/dagster-mvp/pipeline_1.py"
          attribute: repo_1
    ```

    Over time, your codebase will grow. You might introduce support for different encodings. Or, y'know, build an actual ML application. When that happens, you can load more repos from other locations, or even from installed Python packages.

3. **Go through the `dagster` constructs in `pipeline_1.py`**

    There are a few useful ones:

    - a fake `resource` called `connection`: it takes a string `credentials` and turns it to uppercase). In a real use-case, you could use a `resource` for a database connection.
    - 3 `op`s: `get_string()`  selects one of two strings, `normalize_string()` turns an input string into upper or lowercase, and `clean_string` strips an input string of punctuation.
    - a custom `IOManager`: an IOManager is attached to an `op` output, and it runs between the end of an upstream op and the start of a downstream op. This is a slightly tricky concept, but basically Dagster always wants to persist op results to memory - it is concerned that you will lose track of them unless they are saved to disk. The default IOManager pickles the results, whatever they may be. Like civilized ML Engineers, we want to write our string outputs to `.txt`, so we build something called `CustomIOManager` .
    - the resource definitions and configuration needed to turn the `clean_string_graph` into `clean_string_job` using `clean_string_graph.to_job(resource_defs, config)`
    - a `schedule` with the appropriate CRON string to run  `clean_string_job` every minute

## 3 ways to run this pipeline

1. **Run it in Python**

    Start a Python shell in `dagster-mvp` and run:

    ```python
    from pipeline_1 import clean_string_job
    clean_string_job.execute_in_process()
    ```

2. **Run it from the command line**

    ```bash
    dagster job execute clean_string_job
    ```

    If this doesn’t work, double check the env variable `DAGSTER_HOME`.

3. **Run it from a pretty UI**

    Run `dagit` to spin up a pretty local orchestration server.

    ```bash
    dagit
    ```

    Go to [localhost:3000](http://localhost:3000) in your browser.

    You should be able to see your pipelines and jobs. Click on `clean_string_job` and go to the 'Launchpad'. From here, you should be able to do a number of things, like edit the config, run a job, or start a schedule.

4. **(Optional) Play with the scheduler**

    Dagit’s UI also has a Schedules page. Click on it and you’ll see settings that let you turn the scheduler on and off. The scheduler is currently set to run every minute, but you can play with the Cron string in the Schedule definition in `pipeline_1.py`.

    Extra credit assignment: changing code between or during scheduled runs leads to interesting results - play with the orchestrator to see what happens!

## Proof it ran: check the logs

Once you run a job, you will notice some interesting changes to your `dagster-exec` folder.

There will suddenly appear 3 subdirectories, `data`, `logs` and `storage`. If you explore the structure, it should look like this:

```
├── data
│   └── c47d674e-b547-4c81-807a-b2cd823a674b
│       ├── sent.txt
│       └── sent_clean.txt
├── logs
│   ├── events
│   │   ├── c47d674e-b547-4c81-807a-b2cd823a674b.db
│   │   └── index.db
│   ├── runs
│   │   └── runs.db
│   └── schedules
│       └── schedules.db
├── storage
│   └── c47d674e-b547-4c81-807a-b2cd823a674b
│       ├── compute_logs
│       │   ├── clean_string.complete
│       │   ├── clean_string.err
│       │   ├── clean_string.out
│       │   ├── get_string.complete
│       │   ├── get_string.err
│       │   ├── get_string.out
│       │   ├── normalize_string.complete
│       │   ├── normalize_string.err
│       │   └── normalize_string.out
│       └── normalize_string
│           └── sent_norm
```

Here, `c47d674e-b547-4c81-807a-b2cd823a674b` is the `run_id` for your run.

### Logs

Dagster logs pretty much everything about your run in a nice, SQLite `.db` format under `logs`. It also stores `.err`, `.out` and `.complete` compute logs for each op.

`logs` and `storage` are locations set in YAML in `dagster-exec/dagster.yaml`.  You don't need to set them, but then Dagster will store them automatically in the default location.

### IOManagers

The output from `normalize_string` is handled by the default IOManager. That's why it is stored as a pickled object `sent_norm`.

The string outputs from the other 2 of our ops are handled by our custom `CustomIOManager`. Our IOManager stores it in `.txt` files in a dir called `data`, under a sub-dir with same name as the `run_id`.

The IOManager can be useful for finer control on your op outputs (e.g. you can choose which outputs to cache, where to store them, etc.).

## In conclusion

That’s it. This is a minimal Dagster pipeline that just works. If you dig around, you'll find lots of other features I've played with. For example, I am currently uselessly passing an empty dict `hyperparameters` to an op that doesn’t use it.

Two closing points of advice:
- With a tool like Dagster, it's easy to get carried away building non-essential nice-to-haves. Strip your pipeline to the essentials and build the simplest thing that *just works*. You can add complexity and sophistication later, and if anything breaks, always revert to the simpler version that *just works*. But if you start trying to build the *perfect ML code project*, you may struggle with execution. Be **brutal** with cutting out redundant concepts. For example, you don't *really* need the Dagster `resource` concept at all. It's just a nice-to-have, and I used it to configure paths and database connections. But in most cases, it's non-essential.
- The value-add from Dagster for reliable, deployed machine learning should be self-evident. If you have a complicated set of ML pipelines in different repos, Dagster empowers your team to develop  independently but still orchestrate in sync. You can use logic to chain operations using Dagster, launching runs of certain pipelines based on dependencies, periodic `schedules` or even custom `sensor`s that respond to an event. You can leverage multiprocessing to distribute big computations, and wait to collect the results before starting the next step. You can define smart retry conditions and error handling. You can keep your dev process clean by writing simple tests using dummy `config`s for predictable edge-cases.

Hopefully, you can now see the big picture, and have a tangible way to get started. Anything you can do in Python, you can do in Dagster - but also now you can orchestrate it reliably in production.
