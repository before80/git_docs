https://git-scm.com/docs/api-trace2

The Trace2 API can be used to print debug, performance, and telemetry information to stderr or a file. The Trace2 feature is inactive unless explicitly enabled by enabling one or more Trace2 Targets.

The Trace2 API is intended to replace the existing (Trace1) `printf()`-style tracing provided by the existing `GIT_TRACE` and `GIT_TRACE_PERFORMANCE` facilities. During initial implementation, Trace2 and Trace1 may operate in parallel.

The Trace2 API defines a set of high-level messages with known fields, such as (`start`: `argv`) and (`exit`: {`exit-code`, `elapsed-time`}).

Trace2 instrumentation throughout the Git code base sends Trace2 messages to the enabled Trace2 Targets. Targets transform these messages content into purpose-specific formats and write events to their data streams. In this manner, the Trace2 API can drive many different types of analysis.

Targets are defined using a VTable allowing easy extension to other formats in the future. This might be used to define a binary format, for example.

Trace2 is controlled using `trace2.*` config values in the system and global config files and `GIT_TRACE2*` environment variables. Trace2 does not read from repo local or worktree config files, nor does it respect `-c` command line config settings.

## Trace2 Targets

Trace2 defines the following set of Trace2 Targets. Format details are given in a later section.

### The Normal Format Target

The normal format target is a traditional `printf()` format and similar to the `GIT_TRACE` format. This format is enabled with the `GIT_TRACE2` environment variable or the `trace2.normalTarget` system or global config setting.

For example

```
$ export GIT_TRACE2=~/log.normal
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

```
$ git config --global trace2.normalTarget ~/log.normal
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

```
$ cat ~/log.normal
12:28:42.620009 common-main.c:38                  version 2.20.1.155.g426c96fcdb
12:28:42.620989 common-main.c:39                  start git version
12:28:42.621101 git.c:432                         cmd_name version (version)
12:28:42.621215 git.c:662                         exit elapsed:0.001227 code:0
12:28:42.621250 trace2/tr2_tgt_normal.c:124       atexit elapsed:0.001265 code:0
```

### The Performance Format Target

The performance format target (PERF) is a column-based format to replace `GIT_TRACE_PERFORMANCE` and is suitable for development and testing, possibly to complement tools like `gprof`. This format is enabled with the `GIT_TRACE2_PERF` environment variable or the `trace2.perfTarget` system or global config setting.

For example

```
$ export GIT_TRACE2_PERF=~/log.perf
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

```
$ git config --global trace2.perfTarget ~/log.perf
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

```
$ cat ~/log.perf
12:28:42.620675 common-main.c:38                  | d0 | main                     | version      |     |           |           |            | 2.20.1.155.g426c96fcdb
12:28:42.621001 common-main.c:39                  | d0 | main                     | start        |     |  0.001173 |           |            | git version
12:28:42.621111 git.c:432                         | d0 | main                     | cmd_name     |     |           |           |            | version (version)
12:28:42.621225 git.c:662                         | d0 | main                     | exit         |     |  0.001227 |           |            | code:0
12:28:42.621259 trace2/tr2_tgt_perf.c:211         | d0 | main                     | atexit       |     |  0.001265 |           |            | code:0
```

### The Event Format Target

The event format target is a JSON-based format of event data suitable for telemetry analysis. This format is enabled with the `GIT_TRACE2_EVENT` environment variable or the `trace2.eventTarget` system or global config setting.

For example

```
$ export GIT_TRACE2_EVENT=~/log.event
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

```
$ git config --global trace2.eventTarget ~/log.event
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

```
$ cat ~/log.event
{"event":"version","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.620713Z","file":"common-main.c","line":38,"evt":"3","exe":"2.20.1.155.g426c96fcdb"}
{"event":"start","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621027Z","file":"common-main.c","line":39,"t_abs":0.001173,"argv":["git","version"]}
{"event":"cmd_name","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621122Z","file":"git.c","line":432,"name":"version","hierarchy":"version"}
{"event":"exit","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621236Z","file":"git.c","line":662,"t_abs":0.001227,"code":0}
{"event":"atexit","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621268Z","file":"trace2/tr2_tgt_event.c","line":163,"t_abs":0.001265,"code":0}
```

### Enabling a Target

To enable a target, set the corresponding environment variable or system or global config value to one of the following:

- `0` or `false` - Disables the target.
- `1` or `true` - Writes to `STDERR`.
- `[2-9]` - Writes to the already opened file descriptor.
- `<absolute-pathname>` - Writes to the file in append mode. If the target already exists and is a directory, the traces will be written to files (one per process) underneath the given directory.
- `af_unix:[<socket_type>:]<absolute-pathname>` - Write to a Unix DomainSocket (on platforms that support them). Socket type can be either `stream` or `dgram`; if omitted Git will try both.

When trace files are written to a target directory, they will be named according to the last component of the SID (optionally followed by a counter to avoid filename collisions).

## Trace2 API

The Trace2 public API is defined and documented in `trace2.h`; refer to it for more information. All public functions and macros are prefixed with `trace2_` and are implemented in `trace2.c`.

There are no public Trace2 data structures.

The Trace2 code also defines a set of private functions and data types in the `trace2/` directory. These symbols are prefixed with `tr2_` and should only be used by functions in `trace2.c` (or other private source files in `trace2/`).

### Conventions for Public Functions and Macros

Some functions have a `_fl()` suffix to indicate that they take `file` and `line-number` arguments.

Some functions have a `_va_fl()` suffix to indicate that they also take a `va_list` argument.

Some functions have a `_printf_fl()` suffix to indicate that they also take a `printf()` style format with a variable number of arguments.

CPP wrapper macros are defined to hide most of these details.

## Trace2 Target Formats

### NORMAL Format

Events are written as lines of the form:

```
[<time> SP <filename>:<line> SP+] <event-name> [[SP] <event-message>] LF
```

- `<event-name>`

  is the event name.

- `<event-message>`

  is a free-form `printf()` message intended for human consumption.Note that this may contain embedded LF or CRLF characters that are not escaped, so the event may spill across multiple lines.

If `GIT_TRACE2_BRIEF` or `trace2.normalBrief` is true, the `time`, `filename`, and `line` fields are omitted.

This target is intended to be more of a summary (like GIT_TRACE) and less detailed than the other targets. It ignores thread, region, and data messages, for example.

### PERF Format

Events are written as lines of the form:

```
[<time> SP <filename>:<line> SP+
    BAR SP] d<depth> SP
    BAR SP <thread-name> SP+
    BAR SP <event-name> SP+
    BAR SP [r<repo-id>] SP+
    BAR SP [<t_abs>] SP+
    BAR SP [<t_rel>] SP+
    BAR SP [<category>] SP+
    BAR SP DOTS* <perf-event-message>
    LF
```

- `<depth>`

  is the git process depth. This is the number of parent git processes. A top-level git command has depth value "d0". A child of it has depth value "d1". A second level child has depth value "d2" and so on.

- `<thread-name>`

  is a unique name for the thread. The primary thread is called "main". Other thread names are of the form "th%d:%s" and include a unique number and the name of the thread-proc.

- `<event-name>`

  is the event name.

- `<repo-id>`

  when present, is a number indicating the repository in use. A `def_repo` event is emitted when a repository is opened. This defines the repo-id and associated worktree. Subsequent repo-specific events will reference this repo-id.Currently, this is always "r1" for the main repository. This field is in anticipation of in-proc submodules in the future.

- `<t_abs>`

  when present, is the absolute time in seconds since the program started.

- `<t_rel>`

  when present, is time in seconds relative to the start of the current region. For a thread-exit event, it is the elapsed time of the thread.

- `<category>`

  is present on region and data events and is used to indicate a broad category, such as "index" or "status".

- `<perf-event-message>`

  is a free-form `printf()` message intended for human consumption.

```
15:33:33.532712 wt-status.c:2310                  | d0 | main                     | region_enter | r1  |  0.126064 |           | status     | label:print
15:33:33.532712 wt-status.c:2331                  | d0 | main                     | region_leave | r1  |  0.127568 |  0.001504 | status     | label:print
```

If `GIT_TRACE2_PERF_BRIEF` or `trace2.perfBrief` is true, the `time`, `file`, and `line` fields are omitted.

```
d0 | main                     | region_leave | r1  |  0.011717 |  0.009122 | index      | label:preload
```

The PERF target is intended for interactive performance analysis during development and is quite noisy.

### EVENT Format

Each event is a JSON-object containing multiple key/value pairs written as a single line and followed by a LF.

```
'{' <key> ':' <value> [',' <key> ':' <value>]* '}' LF
```

Some key/value pairs are common to all events and some are event-specific.

#### Common Key/Value Pairs

The following key/value pairs are common to all events:

```
{
	"event":"version",
	"sid":"20190408T191827.272759Z-H9b68c35f-P00003510",
	"thread":"main",
	"time":"2019-04-08T19:18:27.282761Z",
	"file":"common-main.c",
	"line":42,
	...
}
```

- `"event":<event>`

  is the event name.

- `"sid":<sid>`

  is the session-id. This is a unique string to identify the process instance to allow all events emitted by a process to be identified. A session-id is used instead of a PID because PIDs are recycled by the OS. For child git processes, the session-id is prepended with the session-id of the parent git process to allow parent-child relationships to be identified during post-processing.

- `"thread":<thread>`

  is the thread name.

- `"time":<time>`

  is the UTC time of the event.

- `"file":<filename>`

  is source file generating the event.

- `"line":<line-number>`

  is the integer source line number generating the event.

- `"repo":<repo-id>`

  when present, is the integer repo-id as described previously.

If `GIT_TRACE2_EVENT_BRIEF` or `trace2.eventBrief` is true, the `file` and `line` fields are omitted from all events and the `time` field is only present on the "start" and "atexit" events.

#### Event-Specific Key/Value Pairs

- `"version"`

  This event gives the version of the executable and the EVENT format. It should always be the first event in a trace session. The EVENT format version will be incremented if new event types are added, if existing fields are removed, or if there are significant changes in interpretation of existing events or fields. Smaller changes, such as adding a new field to an existing event, will not require an increment to the EVENT format version.`{ "event":"version", ... "evt":"3",		       # EVENT format version "exe":"2.20.1.155.g426c96fcdb" # git version }`

- `"too_many_files"`

  This event is written to the git-trace2-discard sentinel file if there are too many files in the target trace directory (see the trace2.maxFiles config option).`{ "event":"too_many_files", ... }`

- `"start"`

  This event contains the complete argv received by main().`{ "event":"start", ... "t_abs":0.001227, # elapsed time in seconds "argv":["git","version"] }`

- `"exit"`

  This event is emitted when git calls `exit()`.`{ "event":"exit", ... "t_abs":0.001227, # elapsed time in seconds "code":0	  # exit code }`

- `"atexit"`

  This event is emitted by the Trace2 `atexit` routine during final shutdown. It should be the last event emitted by the process.(The elapsed time reported here is greater than the time reported in the "exit" event because it runs after all other atexit tasks have completed.)`{ "event":"atexit", ... "t_abs":0.001227, # elapsed time in seconds "code":0          # exit code }`

- `"signal"`

  This event is emitted when the program is terminated by a user signal. Depending on the platform, the signal event may prevent the "atexit" event from being generated.`{ "event":"signal", ... "t_abs":0.001227,  # elapsed time in seconds "signo":13         # SIGTERM, SIGINT, etc. }`

- `"error"`

  This event is emitted when one of the `BUG()`, `bug()`, `error()`, `die()`, `warning()`, or `usage()` functions are called.`{ "event":"error", ... "msg":"invalid option: --cahced", # formatted error message "fmt":"invalid option: %s"	  # error format string }`The error event may be emitted more than once. The format string allows post-processors to group errors by type without worrying about specific error arguments.

- `"cmd_path"`

  This event contains the discovered full path of the git executable (on platforms that are configured to resolve it).`{ "event":"cmd_path", ... "path":"C:/work/gfw/git.exe" }`

- `"cmd_ancestry"`

  This event contains the text command name for the parent (and earlier generations of parents) of the current process, in an array ordered from nearest parent to furthest great-grandparent. It may not be implemented on all platforms.`{ "event":"cmd_ancestry", ... "ancestry":["bash","tmux: server","systemd"] }`

- `"cmd_name"`

  This event contains the command name for this git process and the hierarchy of commands from parent git processes.`{ "event":"cmd_name", ... "name":"pack-objects", "hierarchy":"push/pack-objects" }`Normally, the "name" field contains the canonical name of the command. When a canonical name is not available, one of these special values are used:`"_query_"            # "git --html-path" "_run_dashed_"       # when "git foo" tries to run "git-foo" "_run_shell_alias_"  # alias expansion to a shell command "_run_git_alias_"    # alias expansion to a git command "_usage_"            # usage error`

- `"cmd_mode"`

  This event, when present, describes the command variant. This event may be emitted more than once.`{ "event":"cmd_mode", ... "name":"branch" }`The "name" field is an arbitrary string to describe the command mode. For example, checkout can checkout a branch or an individual file. And these variations typically have different performance characteristics that are not comparable.

- `"alias"`

  This event is present when an alias is expanded.`{ "event":"alias", ... "alias":"l",		 # registered alias "argv":["log","--graph"] # alias expansion }`

- `"child_start"`

  This event describes a child process that is about to be spawned.`{ "event":"child_start", ... "child_id":2, "child_class":"?", "use_shell":false, "argv":["git","rev-list","--objects","--stdin","--not","--all","--quiet"] 	"hook_name":"<hook_name>"  # present when child_class is "hook" "cd":"<path>"		   # present when cd is required }`The "child_id" field can be used to match this child_start with the corresponding child_exit event.The "child_class" field is a rough classification, such as "editor", "pager", "transport/*", and "hook". Unclassified children are classified with "?".

- `"child_exit"`

  This event is generated after the current process has returned from the `waitpid()` and collected the exit information from the child.`{ "event":"child_exit", ... "child_id":2, "pid":14708,	 # child PID "code":0,	 # child exit-code "t_rel":0.110605 # observed run-time of child process }`Note that the session-id of the child process is not available to the current/spawning process, so the child’s PID is reported here as a hint for post-processing. (But it is only a hint because the child process may be a shell script which doesn’t have a session-id.)Note that the `t_rel` field contains the observed run time in seconds for the child process (starting before the fork/exec/spawn and stopping after the `waitpid()` and includes OS process creation overhead). So this time will be slightly larger than the atexit time reported by the child process itself.

- `"child_ready"`

  This event is generated after the current process has started a background process and released all handles to it.`{ "event":"child_ready", ... "child_id":2, "pid":14708,	 # child PID "ready":"ready", # child ready state "t_rel":0.110605 # observed run-time of child process }`Note that the session-id of the child process is not available to the current/spawning process, so the child’s PID is reported here as a hint for post-processing. (But it is only a hint because the child process may be a shell script which doesn’t have a session-id.)This event is generated after the child is started in the background and given a little time to boot up and start working. If the child starts up normally while the parent is still waiting, the "ready" field will have the value "ready". If the child is too slow to start and the parent times out, the field will have the value "timeout". If the child starts but the parent is unable to probe it, the field will have the value "error".After the parent process emits this event, it will release all of its handles to the child process and treat the child as a background daemon. So even if the child does eventually finish booting up, the parent will not emit an updated event.Note that the `t_rel` field contains the observed run time in seconds when the parent released the child process into the background. The child is assumed to be a long-running daemon process and may outlive the parent process. So the parent’s child event times should not be compared to the child’s atexit times.

- `"exec"`

  This event is generated before git attempts to `exec()` another command rather than starting a child process.`{ "event":"exec", ... "exec_id":0, "exe":"git", "argv":["foo", "bar"] }`The "exec_id" field is a command-unique id and is only useful if the `exec()` fails and a corresponding exec_result event is generated.

- `"exec_result"`

  This event is generated if the `exec()` fails and control returns to the current git command.`{ "event":"exec_result", ... "exec_id":0, "code":1      # error code (errno) from exec() }`

- `"thread_start"`

  This event is generated when a thread is started. It is generated from **within** the new thread’s thread-proc (because it needs to access data in the thread’s thread-local storage).`{ "event":"thread_start", ... "thread":"th02:preload_thread" # thread name }`

- `"thread_exit"`

  This event is generated when a thread exits. It is generated from **within** the thread’s thread-proc.`{ "event":"thread_exit", ... "thread":"th02:preload_thread", # thread name "t_rel":0.007328                # thread elapsed time }`

- `"def_param"`

  This event is generated to log a global parameter, such as a config setting, command-line flag, or environment variable.`{ "event":"def_param", ... "scope":"global", "param":"core.abbrev", "value":"7" }`

- `"def_repo"`

  This event defines a repo-id and associates it with the root of the worktree.`{ "event":"def_repo", ... "repo":1, "worktree":"/Users/jeffhost/work/gfw" }`As stated earlier, the repo-id is currently always 1, so there will only be one def_repo event. Later, if in-proc submodules are supported, a def_repo event should be emitted for each submodule visited.

- `"region_enter"`

  This event is generated when entering a region.`{ "event":"region_enter", ... "repo":1,                # optional "nesting":1,             # current region stack depth "category":"index",      # optional "label":"do_read_index", # optional "msg":".git/index"       # optional }`The `category` field may be used in a future enhancement to do category-based filtering.`GIT_TRACE2_EVENT_NESTING` or `trace2.eventNesting` can be used to filter deeply nested regions and data events. It defaults to "2".

- `"region_leave"`

  This event is generated when leaving a region.`{ "event":"region_leave", ... "repo":1,                # optional "t_rel":0.002876,        # time spent in region in seconds "nesting":1,             # region stack depth "category":"index",      # optional "label":"do_read_index", # optional "msg":".git/index"       # optional }`

- `"data"`

  This event is generated to log a thread- and region-local key/value pair.`{ "event":"data", ... "repo":1,              # optional "t_abs":0.024107,      # absolute elapsed time "t_rel":0.001031,      # elapsed time in region/thread "nesting":2,           # region stack depth "category":"index", "key":"read/cache_nr", "value":"3552" }`The "value" field may be an integer or a string.

- `"data-json"`

  This event is generated to log a pre-formatted JSON string containing structured data.`{ "event":"data_json", ... "repo":1,              # optional "t_abs":0.015905, "t_rel":0.015905, "nesting":1, "category":"process", "key":"windows/ancestry", "value":["bash.exe","bash.exe"] }`

- `"th_timer"`

  This event logs the amount of time that a stopwatch timer was running in the thread. This event is generated when a thread exits for timers that requested per-thread events.`{ "event":"th_timer", ... "category":"my_category", "name":"my_timer", "intervals":5,         # number of time it was started/stopped "t_total":0.052741,    # total time in seconds it was running "t_min":0.010061,      # shortest interval "t_max":0.011648       # longest interval }`

- `"timer"`

  This event logs the amount of time that a stopwatch timer was running aggregated across all threads. This event is generated when the process exits.`{ "event":"timer", ... "category":"my_category", "name":"my_timer", "intervals":5,         # number of time it was started/stopped "t_total":0.052741,    # total time in seconds it was running "t_min":0.010061,      # shortest interval "t_max":0.011648       # longest interval }`

- `"th_counter"`

  This event logs the value of a counter variable in a thread. This event is generated when a thread exits for counters that requested per-thread events.`{ "event":"th_counter", ... "category":"my_category", "name":"my_counter", "count":23 }`

- `"counter"`

  This event logs the value of a counter variable across all threads. This event is generated when the process exits. The total value reported here is the sum across all threads.`{ "event":"counter", ... "category":"my_category", "name":"my_counter", "count":23 }`

## Example Trace2 API Usage

Here is a hypothetical usage of the Trace2 API showing the intended usage (without worrying about the actual Git details).

- Initialization

  Initialization happens in `main()`. Behind the scenes, an `atexit` and `signal` handler are registered.`int main(int argc, const char **argv) { int exit_code; 	trace2_initialize(); trace2_cmd_start(argv); 	exit_code = cmd_main(argc, argv); 	trace2_cmd_exit(exit_code); 	return exit_code; }`

- Command Details

  After the basics are established, additional command information can be sent to Trace2 as it is discovered.`int cmd_checkout(int argc, const char **argv) { trace2_cmd_name("checkout"); trace2_cmd_mode("branch"); trace2_def_repo(the_repository); 	// emit "def_param" messages for "interesting" config settings. trace2_cmd_list_config(); 	if (do_something())     trace2_cmd_error("Path '%s': cannot do something", path); 	return 0; }`

- Child Processes

  Wrap code spawning child processes.`void run_child(...) { int child_exit_code; struct child_process cmd = CHILD_PROCESS_INIT; ... cmd.trace2_child_class = "editor"; 	trace2_child_start(&cmd); child_exit_code = spawn_child_and_wait_for_it(); trace2_child_exit(&cmd, child_exit_code); }`For example, the following fetch command spawned ssh, index-pack, rev-list, and gc. This example also shows that fetch took 5.199 seconds and of that 4.932 was in ssh.`$ export GIT_TRACE2_BRIEF=1 $ export GIT_TRACE2=~/log.normal $ git fetch origin ...``$ cat ~/log.normal version 2.20.1.vfs.1.1.47.g534dbe1ad1 start git fetch origin worktree /Users/jeffhost/work/gfw cmd_name fetch (fetch) child_start[0] ssh git@github.com ... child_start[1] git index-pack ... ... (Trace2 events from child processes omitted) child_exit[1] pid:14707 code:0 elapsed:0.076353 child_exit[0] pid:14706 code:0 elapsed:4.931869 child_start[2] git rev-list ... ... (Trace2 events from child process omitted) child_exit[2] pid:14708 code:0 elapsed:0.110605 child_start[3] git gc --auto ... (Trace2 events from child process omitted) child_exit[3] pid:14709 code:0 elapsed:0.006240 exit elapsed:5.198503 code:0 atexit elapsed:5.198541 code:0`When a git process is a (direct or indirect) child of another git process, it inherits Trace2 context information. This allows the child to print the command hierarchy. This example shows gc as child[3] of fetch. When the gc process reports its name as "gc", it also reports the hierarchy as "fetch/gc". (In this example, trace2 messages from the child process is indented for clarity.)`$ export GIT_TRACE2_BRIEF=1 $ export GIT_TRACE2=~/log.normal $ git fetch origin ...``$ cat ~/log.normal version 2.20.1.160.g5676107ecd.dirty start git fetch official worktree /Users/jeffhost/work/gfw cmd_name fetch (fetch) ... child_start[3] git gc --auto    version 2.20.1.160.g5676107ecd.dirty    start /Users/jeffhost/work/gfw/git gc --auto    worktree /Users/jeffhost/work/gfw    cmd_name gc (fetch/gc)    exit elapsed:0.001959 code:0    atexit elapsed:0.001997 code:0 child_exit[3] pid:20303 code:0 elapsed:0.007564 exit elapsed:3.868938 code:0 atexit elapsed:3.868970 code:0`

- Regions

  Regions can be used to time an interesting section of code.`void wt_status_collect(struct wt_status *s) { trace2_region_enter("status", "worktrees", s->repo); wt_status_collect_changes_worktree(s); trace2_region_leave("status", "worktrees", s->repo); 	trace2_region_enter("status", "index", s->repo); wt_status_collect_changes_index(s); trace2_region_leave("status", "index", s->repo); 	trace2_region_enter("status", "untracked", s->repo); wt_status_collect_untracked(s); trace2_region_leave("status", "untracked", s->repo); } void wt_status_print(struct wt_status *s) { trace2_region_enter("status", "print", s->repo); switch (s->status_format) {     ... } trace2_region_leave("status", "print", s->repo); }`In this example, scanning for untracked files ran from +0.012568 to +0.027149 (since the process started) and took 0.014581 seconds.`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ git status ... $ cat ~/log.perf d0 | main                     | version      |     |           |           |            | 2.20.1.160.g5676107ecd.dirty d0 | main                     | start        |     |  0.001173 |           |            | git status d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw d0 | main                     | cmd_name     |     |           |           |            | status (status) ... d0 | main                     | region_enter | r1  |  0.010988 |           | status     | label:worktrees d0 | main                     | region_leave | r1  |  0.011236 |  0.000248 | status     | label:worktrees d0 | main                     | region_enter | r1  |  0.011260 |           | status     | label:index d0 | main                     | region_leave | r1  |  0.012542 |  0.001282 | status     | label:index d0 | main                     | region_enter | r1  |  0.012568 |           | status     | label:untracked d0 | main                     | region_leave | r1  |  0.027149 |  0.014581 | status     | label:untracked d0 | main                     | region_enter | r1  |  0.027411 |           | status     | label:print d0 | main                     | region_leave | r1  |  0.028741 |  0.001330 | status     | label:print d0 | main                     | exit         |     |  0.028778 |           |            | code:0 d0 | main                     | atexit       |     |  0.028809 |           |            | code:0`Regions may be nested. This causes messages to be indented in the PERF target, for example. Elapsed times are relative to the start of the corresponding nesting level as expected. For example, if we add region message to:`static enum path_treatment read_directory_recursive(struct dir_struct *dir, struct index_state *istate, const char *base, int baselen, struct untracked_cache_dir *untracked, int check_only, int stop_at_first_file, const struct pathspec *pathspec) { enum path_treatment state, subdir_state, dir_state = path_none; 	trace2_region_enter_printf("dir", "read_recursive", NULL, "%.*s", baselen, base); ... trace2_region_leave_printf("dir", "read_recursive", NULL, "%.*s", baselen, base); return dir_state; }`We can further investigate the time spent scanning for untracked files.`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ git status ... $ cat ~/log.perf d0 | main                     | version      |     |           |           |            | 2.20.1.162.gb4ccea44db.dirty d0 | main                     | start        |     |  0.001173 |           |            | git status d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw d0 | main                     | cmd_name     |     |           |           |            | status (status) ... d0 | main                     | region_enter | r1  |  0.015047 |           | status     | label:untracked d0 | main                     | region_enter |     |  0.015132 |           | dir        | ..label:read_recursive d0 | main                     | region_enter |     |  0.016341 |           | dir        | ....label:read_recursive vcs-svn/ d0 | main                     | region_leave |     |  0.016422 |  0.000081 | dir        | ....label:read_recursive vcs-svn/ d0 | main                     | region_enter |     |  0.016446 |           | dir        | ....label:read_recursive xdiff/ d0 | main                     | region_leave |     |  0.016522 |  0.000076 | dir        | ....label:read_recursive xdiff/ d0 | main                     | region_enter |     |  0.016612 |           | dir        | ....label:read_recursive git-gui/ d0 | main                     | region_enter |     |  0.016698 |           | dir        | ......label:read_recursive git-gui/po/ d0 | main                     | region_enter |     |  0.016810 |           | dir        | ........label:read_recursive git-gui/po/glossary/ d0 | main                     | region_leave |     |  0.016863 |  0.000053 | dir        | ........label:read_recursive git-gui/po/glossary/ ... d0 | main                     | region_enter |     |  0.031876 |           | dir        | ....label:read_recursive builtin/ d0 | main                     | region_leave |     |  0.032270 |  0.000394 | dir        | ....label:read_recursive builtin/ d0 | main                     | region_leave |     |  0.032414 |  0.017282 | dir        | ..label:read_recursive d0 | main                     | region_leave | r1  |  0.032454 |  0.017407 | status     | label:untracked ... d0 | main                     | exit         |     |  0.034279 |           |            | code:0 d0 | main                     | atexit       |     |  0.034322 |           |            | code:0`Trace2 regions are similar to the existing trace_performance_enter() and trace_performance_leave() routines, but are thread safe and maintain per-thread stacks of timers.

- Data Messages

  Data messages added to a region.`int read_index_from(struct index_state *istate, const char *path, const char *gitdir) { trace2_region_enter_printf("index", "do_read_index", the_repository, "%s", path); 	... 	trace2_data_intmax("index", the_repository, "read/version", istate->version); trace2_data_intmax("index", the_repository, "read/cache_nr", istate->cache_nr); 	trace2_region_leave_printf("index", "do_read_index", the_repository, "%s", path); }`This example shows that the index contained 3552 entries.`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ git status ... $ cat ~/log.perf d0 | main                     | version      |     |           |           |            | 2.20.1.156.gf9916ae094.dirty d0 | main                     | start        |     |  0.001173 |           |            | git status d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw d0 | main                     | cmd_name     |     |           |           |            | status (status) d0 | main                     | region_enter | r1  |  0.001791 |           | index      | label:do_read_index .git/index d0 | main                     | data         | r1  |  0.002494 |  0.000703 | index      | ..read/version:2 d0 | main                     | data         | r1  |  0.002520 |  0.000729 | index      | ..read/cache_nr:3552 d0 | main                     | region_leave | r1  |  0.002539 |  0.000748 | index      | label:do_read_index .git/index ...`

- Thread Events

  Thread messages added to a thread-proc.For example, the multi-threaded preload-index code can be instrumented with a region around the thread pool and then per-thread start and exit events within the thread-proc.`static void *preload_thread(void *_data) { // start the per-thread clock and emit a message. trace2_thread_start("preload_thread"); 	// report which chunk of the array this thread was assigned. trace2_data_intmax("index", the_repository, "offset", p->offset); trace2_data_intmax("index", the_repository, "count", nr); 	do {     ... } while (--nr > 0); ... 	// report elapsed time taken by this thread. trace2_thread_exit(); return NULL; } void preload_index(struct index_state *index, const struct pathspec *pathspec, unsigned int refresh_flags) { trace2_region_enter("index", "preload", the_repository); 	for (i = 0; i < threads; i++) {     ... /* create thread */ } 	for (i = 0; i < threads; i++) {     ... /* join thread */ } 	trace2_region_leave("index", "preload", the_repository); }`In this example preload_index() was executed by the `main` thread and started the `preload` region. Seven threads, named `th01:preload_thread` through `th07:preload_thread`, were started. Events from each thread are atomically appended to the shared target stream as they occur so they may appear in random order with respect other threads. Finally, the main thread waits for the threads to finish and leaves the region.Data events are tagged with the active thread name. They are used to report the per-thread parameters.`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ git status ... $ cat ~/log.perf ... d0 | main                     | region_enter | r1  |  0.002595 |           | index      | label:preload d0 | th01:preload_thread      | thread_start |     |  0.002699 |           |            | d0 | th02:preload_thread      | thread_start |     |  0.002721 |           |            | d0 | th01:preload_thread      | data         | r1  |  0.002736 |  0.000037 | index      | offset:0 d0 | th02:preload_thread      | data         | r1  |  0.002751 |  0.000030 | index      | offset:2032 d0 | th03:preload_thread      | thread_start |     |  0.002711 |           |            | d0 | th06:preload_thread      | thread_start |     |  0.002739 |           |            | d0 | th01:preload_thread      | data         | r1  |  0.002766 |  0.000067 | index      | count:508 d0 | th06:preload_thread      | data         | r1  |  0.002856 |  0.000117 | index      | offset:2540 d0 | th03:preload_thread      | data         | r1  |  0.002824 |  0.000113 | index      | offset:1016 d0 | th04:preload_thread      | thread_start |     |  0.002710 |           |            | d0 | th02:preload_thread      | data         | r1  |  0.002779 |  0.000058 | index      | count:508 d0 | th06:preload_thread      | data         | r1  |  0.002966 |  0.000227 | index      | count:508 d0 | th07:preload_thread      | thread_start |     |  0.002741 |           |            | d0 | th07:preload_thread      | data         | r1  |  0.003017 |  0.000276 | index      | offset:3048 d0 | th05:preload_thread      | thread_start |     |  0.002712 |           |            | d0 | th05:preload_thread      | data         | r1  |  0.003067 |  0.000355 | index      | offset:1524 d0 | th05:preload_thread      | data         | r1  |  0.003090 |  0.000378 | index      | count:508 d0 | th07:preload_thread      | data         | r1  |  0.003037 |  0.000296 | index      | count:504 d0 | th03:preload_thread      | data         | r1  |  0.002971 |  0.000260 | index      | count:508 d0 | th04:preload_thread      | data         | r1  |  0.002983 |  0.000273 | index      | offset:508 d0 | th04:preload_thread      | data         | r1  |  0.007311 |  0.004601 | index      | count:508 d0 | th05:preload_thread      | thread_exit  |     |  0.008781 |  0.006069 |            | d0 | th01:preload_thread      | thread_exit  |     |  0.009561 |  0.006862 |            | d0 | th03:preload_thread      | thread_exit  |     |  0.009742 |  0.007031 |            | d0 | th06:preload_thread      | thread_exit  |     |  0.009820 |  0.007081 |            | d0 | th02:preload_thread      | thread_exit  |     |  0.010274 |  0.007553 |            | d0 | th07:preload_thread      | thread_exit  |     |  0.010477 |  0.007736 |            | d0 | th04:preload_thread      | thread_exit  |     |  0.011657 |  0.008947 |            | d0 | main                     | region_leave | r1  |  0.011717 |  0.009122 | index      | label:preload ... d0 | main                     | exit         |     |  0.029996 |           |            | code:0 d0 | main                     | atexit       |     |  0.030027 |           |            | code:0`In this example, the preload region took 0.009122 seconds. The 7 threads took between 0.006069 and 0.008947 seconds to work on their portion of the index. Thread "th01" worked on 508 items at offset 0. Thread "th02" worked on 508 items at offset 2032. Thread "th04" worked on 508 items at offset 508.This example also shows that thread names are assigned in a racy manner as each thread starts.

- Config (def param) Events

  Dump "interesting" config values to trace2 log.We can optionally emit configuration events, see `trace2.configparams` in [git-config[1]](https://git-scm.com/docs/git-config) for how to enable it.`$ git config --system color.ui never $ git config --global color.ui always $ git config --local color.ui auto $ git config --list --show-scope | grep 'color.ui' system  color.ui=never global  color.ui=always local   color.ui=auto`Then, mark the config `color.ui` as "interesting" config with `GIT_TRACE2_CONFIG_PARAMS`:`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ export GIT_TRACE2_CONFIG_PARAMS=color.ui $ git version ... $ cat ~/log.perf d0 | main                     | version      |     |           |           |              | ... d0 | main                     | start        |     |  0.001642 |           |              | /usr/local/bin/git version d0 | main                     | cmd_name     |     |           |           |              | version (version) d0 | main                     | def_param    |     |           |           | scope:system | color.ui:never d0 | main                     | def_param    |     |           |           | scope:global | color.ui:always d0 | main                     | def_param    |     |           |           | scope:local  | color.ui:auto d0 | main                     | data         | r0  |  0.002100 |  0.002100 | fsync        | fsync/writeout-only:0 d0 | main                     | data         | r0  |  0.002126 |  0.002126 | fsync        | fsync/hardware-flush:0 d0 | main                     | exit         |     |  0.000470 |           |              | code:0 d0 | main                     | atexit       |     |  0.000477 |           |              | code:0`

- Stopwatch Timer Events

  Measure the time spent in a function call or span of code that might be called from many places within the code throughout the life of the process.`static void expensive_function(void) { trace2_timer_start(TRACE2_TIMER_ID_TEST1); ... sleep_millisec(1000); // Do something expensive ... trace2_timer_stop(TRACE2_TIMER_ID_TEST1); } static int ut_100timer(int argc, const char **argv) { ... 	expensive_function(); 	// Do something else 1... 	expensive_function(); 	// Do something else 2... 	expensive_function(); 	return 0; }`In this example, we measure the total time spent in `expensive_function()` regardless of when it is called in the overall flow of the program.`$ export GIT_TRACE2_PERF_BRIEF=1 $ export GIT_TRACE2_PERF=~/log.perf $ t/helper/test-tool trace2 100timer 3 1000 ... $ cat ~/log.perf d0 | main                     | version      |     |           |           |              | ... d0 | main                     | start        |     |  0.001453 |           |              | t/helper/test-tool trace2 100timer 3 1000 d0 | main                     | cmd_name     |     |           |           |              | trace2 (trace2) d0 | main                     | exit         |     |  3.003667 |           |              | code:0 d0 | main                     | timer        |     |           |           | test         | name:test1 intervals:3 total:3.001686 min:1.000254 max:1.000929 d0 | main                     | atexit       |     |  3.003796 |           |              | code:0`

## Future Work

### Relationship to the Existing Trace Api (api-trace.txt)

There are a few issues to resolve before we can completely switch to Trace2.

- Updating existing tests that assume `GIT_TRACE` format messages.
- How to best handle custom `GIT_TRACE_<key>` messages?
  - The `GIT_TRACE_<key>` mechanism allows each <key> to write to a different file (in addition to just stderr).
  - Do we want to maintain that ability or simply write to the existing Trace2 targets (and convert <key> to a "category").