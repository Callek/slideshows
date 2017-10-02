---

# Taskgraph Transforms
## Releng Tech Talk
### Oct 2017

---

## Where we'll go..

 - What are the stages of taskgraph generation
 -- and how can you test them locally |
 - What are and how do I work with 'transforms'

---

## Taskgraph is... |
## Like a grocery store.. |
### Yes Really...

+++

Suppose you want to make a meal, including baking a cake, so you want to go to
the grocery store.

 - The stores contents, itself, is your 'full' taskgraph. |
 - Your shopping list (including: cake mix) is considered your 'target' tasks |
 - The items you bring up to the register is your *optimized* task list. |
 - Any substitutions you make at home, would be your final task list ('morphed') |

Lets go into those in a bit more detail.

+++

*Full* task set:

 - In the grocery store you have available to you everything you are able to buy. |
 - In the taskgraph the full graph is similar, where all tasks you could possibly run
are generated and represented at the end of this stage. |

The taskgraph fills the shelves in the *full* stage.

+++

*Target* task set:

 - When you walk into the grocery store, you usually know what you want by having a list. The items you want are stuff you know the store to be selling. So you walk the aisles to pick out what you need. |
 - In the taskgraph your target set is all the tasks you explicitly want to run, ignoring anything that may need to run to support them. |
   - This is most evident for try pushes (where your target set could be just a single test) or on Nightlies where the target set could be all android nightly tasks. |

The target set must be part of the *full* taskgraph, and can be different depending
on what files were changed in 'this' push, and what is being run (nightly, valgrind,
etc), as well as could vary by project.

+++

*Optimized* task set:

- When you are in the grocery store, after you pick up the cake mix, you see that
you need to buy eggs and frosting for your cake. So you pick those up as well, even
though they were not on your shopping list. You also realize it called for buttermilk,
but you think you have some at home already, so you omit buying it. |
- The optimized task set works similarly. |
-- It will make sure all its dependencies are selected (builds needed for the
selected tests get run) |
-- It will also try and avoid doing unnecessary work, for example it will avoid
running docker-worker tasks if there was already a known one that succeeded.

---

# Testing Locally

+++

```shell
$ ./mach taskgraph --help
usage: mach [global arguments] taskgraph subcommand [subcommand arguments]

The taskgraph subcommands all relate to the generation of task graphs
for Gecko continuous integration.  A task graph is a set of tasks linked
by dependencies: for example, a binary must be built before it is tested,
and that build may further depend on various toolchains, libraries, etc.

Global Arguments:
  -v, --verbose         Print verbose output.
  -l FILENAME, --log-file FILENAME
                        Filename to write log data to.
  --log-interval        Prefix log line with interval from last message rather
                        than relative time. Note that this is NOT execution
                        time if there are parallel operations.
  --log-no-times        Do not prefix log lines with times. By default, mach
                        will prefix each output line with the time since
                        command start.
  -h, --help            Show this help message.
  --debug-command       Start a Python debugger when command is dispatched.
  --settings FILENAME   Path to settings file.

Sub Commands:
  action-callback       Run action callback used by action tasks
  action-task           Run the add-tasks task. DEPRECATED! Use 'add-tasks'
                        instead.
  add-talos             Run the add-talos task
  add-tasks             Run the add-tasks task
  backfill              Run the backfill task
  cron                  Run the cron task
  decision              Run the decision task
  full                  Show the full taskgraph
  morphed               Show the morphed taskgraph
  optimized             Show the optimized taskgraph
  target                Show the target task set
  target-graph          Show the target taskgraph
  tasks                 Show all tasks in the taskgraph
  test-action-callback  Run an action callback in a testing mode
```


---