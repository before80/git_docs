+++
title = "API Trace2"
weight = 30
type = "docs"
date = 2023-07-30T10:20:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# API Trace2

https://git-scm.com/docs/api-trace2



The Trace2 API can be used to print debug, performance, and telemetry information to stderr or a file. The Trace2 feature is inactive unless explicitly enabled by enabling one or more Trace2 Targets.

​	Trace2 API可以用于将调试、性能和遥测信息打印到stderr或文件中。除非明确启用一个或多个Trace2目标，否则Trace2功能是不活动的。

The Trace2 API is intended to replace the existing (Trace1) `printf()`-style tracing provided by the existing `GIT_TRACE` and `GIT_TRACE_PERFORMANCE` facilities. During initial implementation, Trace2 and Trace1 may operate in parallel.

​	Trace2 API旨在取代现有（Trace1）的`printf()`样式跟踪，这些跟踪由现有的`GIT_TRACE`和`GIT_TRACE_PERFORMANCE`功能提供。在最初实现时，Trace2和Trace1可能会并行操作。

The Trace2 API defines a set of high-level messages with known fields, such as (`start`: `argv`) and (`exit`: {`exit-code`, `elapsed-time`}).

​	Trace2 API定义了一组具有已知字段的高级消息，例如（`start`: `argv`）和（`exit`: {`exit-code`, `elapsed-time`}）。

Trace2 instrumentation throughout the Git code base sends Trace2 messages to the enabled Trace2 Targets. Targets transform these messages content into purpose-specific formats and write events to their data streams. In this manner, the Trace2 API can drive many different types of analysis.

​	Git代码库中的Trace2插装会将Trace2消息发送到已启用的Trace2目标。目标将这些消息内容转换为特定目的的格式，并将事件写入其数据流。通过这种方式，Trace2 API可以驱动许多不同类型的分析。

Targets are defined using a VTable allowing easy extension to other formats in the future. This might be used to define a binary format, for example.

​	目标使用VTable定义，允许将来轻松扩展到其他格式。例如，这可以用于定义二进制格式。

Trace2 is controlled using `trace2.*` config values in the system and global config files and `GIT_TRACE2*` environment variables. Trace2 does not read from repo local or worktree config files, nor does it respect `-c` command line config settings.

​	Trace2使用系统和全局配置文件中的`trace2.*`配置值以及`GIT_TRACE2*`环境变量进行控制。Trace2不会从仓库本地或工作树配置文件中读取，也不会遵循`-c`命令行配置设置。

## Trace2 目标

Trace2 defines the following set of Trace2 Targets. Format details are given in a later section.

​	Trace2定义了以下一组Trace2目标。格式详细信息在后面的部分中给出。

### 普通格式目标

The normal format target is a traditional `printf()` format and similar to the `GIT_TRACE` format. This format is enabled with the `GIT_TRACE2` environment variable or the `trace2.normalTarget` system or global config setting.

​	普通格式目标是传统的`printf()`格式，类似于`GIT_TRACE`格式。可以通过`GIT_TRACE2`环境变量或`trace2.normalTarget`系统或全局配置设置启用此格式。

For example

​	例如：

```
$ export GIT_TRACE2=~/log.normal
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

或

```
$ git config --global trace2.normalTarget ~/log.normal
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

会输出：

```
$ cat ~/log.normal
12:28:42.620009 common-main.c:38                  version 2.20.1.155.g426c96fcdb
12:28:42.620989 common-main.c:39                  start git version
12:28:42.621101 git.c:432                         cmd_name version (version)
12:28:42.621215 git.c:662                         exit elapsed:0.001227 code:0
12:28:42.621250 trace2/tr2_tgt_normal.c:124       atexit elapsed:0.001265 code:0
```

### 性能格式目标

The performance format target (PERF) is a column-based format to replace `GIT_TRACE_PERFORMANCE` and is suitable for development and testing, possibly to complement tools like `gprof`. This format is enabled with the `GIT_TRACE2_PERF` environment variable or the `trace2.perfTarget` system or global config setting.

​	性能格式目标（PERF）是一种基于列的格式，用于取代`GIT_TRACE_PERFORMANCE`，适用于开发和测试，可能与`gprof`等工具相补充。可以通过`GIT_TRACE2_PERF`环境变量或`trace2.perfTarget`系统或全局配置设置启用此格式。

For example

​	例如：

```
$ export GIT_TRACE2_PERF=~/log.perf
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

或

```
$ git config --global trace2.perfTarget ~/log.perf
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

会输出：

```
$ cat ~/log.perf
12:28:42.620675 common-main.c:38                  | d0 | main                     | version      |     |           |           |            | 2.20.1.155.g426c96fcdb
12:28:42.621001 common-main.c:39                  | d0 | main                     | start        |     |  0.001173 |           |            | git version
12:28:42.621111 git.c:432                         | d0 | main                     | cmd_name     |     |           |           |            | version (version)
12:28:42.621225 git.c:662                         | d0 | main                     | exit         |     |  0.001227 |           |            | code:0
12:28:42.621259 trace2/tr2_tgt_perf.c:211         | d0 | main                     | atexit       |     |  0.001265 |           |            | code:0
```

### 事件格式目标

The event format target is a JSON-based format of event data suitable for telemetry analysis. This format is enabled with the `GIT_TRACE2_EVENT` environment variable or the `trace2.eventTarget` system or global config setting.

​	事件格式目标是适用于遥测分析的基于JSON的事件数据格式。可以通过`GIT_TRACE2_EVENT`环境变量或`trace2.eventTarget`系统或全局配置设置启用此格式。

For example

​	例如：

```
$ export GIT_TRACE2_EVENT=~/log.event
$ git version
git version 2.20.1.155.g426c96fcdb
```

or

或

```
$ git config --global trace2.eventTarget ~/log.event
$ git version
git version 2.20.1.155.g426c96fcdb
```

yields

会输出：

```
$ cat ~/log.event
{"event":"version","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.620713Z","file":"common-main.c","line":38,"evt":"3","exe":"2.20.1.155.g426c96fcdb"}
{"event":"start","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621027Z","file":"common-main.c","line":39,"t_abs":0.001173,"argv":["git","version"]}
{"event":"cmd_name","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621122Z","file":"git.c","line":432,"name":"version","hierarchy":"version"}
{"event":"exit","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621236Z","file":"git.c","line":662,"t_abs":0.001227,"code":0}
{"event":"atexit","sid":"20190408T191610.507018Z-H9b68c35f-P000059a8","thread":"main","time":"2019-01-16T17:28:42.621268Z","file":"trace2/tr2_tgt_event.c","line":163,"t_abs":0.001265,"code":0}
```

### 启用目标

To enable a target, set the corresponding environment variable or system or global config value to one of the following:

​	要启用目标，请将相应的环境变量或系统或全局配置值设置为以下之一：

- `0` or `false` - Disables the target.
- `0`或`false` - 禁用目标。
- `1` or `true` - Writes to `STDERR`.
- `1`或`true` - 写入`STDERR`。
- `[2-9]` - Writes to the already opened file descriptor.
- `[2-9]` - 写入已打开的文件描述符。
- `<absolute-pathname>` - Writes to the file in append mode. If the target already exists and is a directory, the traces will be written to files (one per process) underneath the given directory.
- `<绝对路径名>` - 以追加模式写入文件。如果目标已存在且为目录，则跟踪将被写入给定目录下的文件（每个进程一个文件）。
- `af_unix:[<socket_type>:]<absolute-pathname>` - Write to a Unix DomainSocket (on platforms that support them). Socket type can be either `stream` or `dgram`; if omitted Git will try both.
- `af_unix:[<socket_type>:]<绝对路径名>` - 写入Unix DomainSocket（仅适用于支持它们的平台）。套接字类型可以是`stream`或`dgram`；如果省略，则Git将尝试两种类型。

When trace files are written to a target directory, they will be named according to the last component of the SID (optionally followed by a counter to avoid filename collisions).

​	当跟踪文件被写入目标目录时，它们的命名将根据SID的最后一个组件命名（可选地跟随一个计数器，以避免文件名冲突）。

## Trace2 API

The Trace2 public API is defined and documented in `trace2.h`; refer to it for more information. All public functions and macros are prefixed with `trace2_` and are implemented in `trace2.c`.

​	Trace2公共API在`trace2.h`中定义和记录。有关更多信息，请参阅该文件。所有公共函数和宏都以`trace2_`为前缀，并在`trace2.c`中实现。

There are no public Trace2 data structures.

​	没有公共的Trace2数据结构。

The Trace2 code also defines a set of private functions and data types in the `trace2/` directory. These symbols are prefixed with `tr2_` and should only be used by functions in `trace2.c` (or other private source files in `trace2/`).

​	Trace2代码还在`trace2/`目录中定义了一组私有函数和数据类型。这些符号以`tr2_`为前缀，并且只应该由`trace2.c`中的函数（或`trace2/`中的其他私有源文件）使用。

### 公共函数和宏的约定

Some functions have a `_fl()` suffix to indicate that they take `file` and `line-number` arguments.

​	某些函数具有`_fl()`后缀，表示它们需要`file`和`line-number`参数。

Some functions have a `_va_fl()` suffix to indicate that they also take a `va_list` argument.

​	某些函数具有`_va_fl()`后缀，表示它们还需要一个`va_list`参数。

Some functions have a `_printf_fl()` suffix to indicate that they also take a `printf()` style format with a variable number of arguments.

​	某些函数具有`_printf_fl()`后缀，表示它们还接受`printf()`样式格式和可变数量的参数。

CPP wrapper macros are defined to hide most of these details.

​	定义了CPP包装器宏，用于隐藏大部分这些细节。

## Trace2 目标格式

### 普通格式

Events are written as lines of the form:

​	事件被写入以下形式的行：

```
[<time> SP <filename>:<line> SP+] <event-name> [[SP] <event-message>] LF
```

- `<event-name>`: is the event name.
- `<事件名称>` 是事件名称。
- `<event-message>`: is a free-form `printf()` message intended for human consumption.Note that this may contain embedded LF or CRLF characters that are not escaped, so the event may spill across multiple lines.
- `<事件消息>` 是供人类阅读的自由格式的`printf()`消息。请注意，此消息可能包含未转义的嵌入式LF或CRLF字符，因此事件可能跨越多行。

If `GIT_TRACE2_BRIEF` or `trace2.normalBrief` is true, the `time`, `filename`, and `line` fields are omitted.

​	如果`GIT_TRACE2_BRIEF`或`trace2.normalBrief`为true，则省略`时间`，`文件名`和`行号`字段。

This target is intended to be more of a summary (like GIT_TRACE) and less detailed than the other targets. It ignores thread, region, and data messages, for example.

​	该目标用于更像是摘要（类似于GIT_TRACE），而不是其他目标那样详细。例如，它会忽略线程、区域和数据消息。

### PERF 格式

Events are written as lines of the form:

​	事件被写入以下形式的行：

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

- `<depth>`: is the git process depth. This is the number of parent git processes. A top-level git command has depth value "d0". A child of it has depth value "d1". A second level child has depth value "d2" and so on.
- `<depth>` 是git进程深度。这是父git进程的数量。顶层git命令的深度值为“d0”。其子级的深度值为“d1”。第二级子级的深度值为“d2”，以此类推。
- `<thread-name>`: is a unique name for the thread. The primary thread is called "main". Other thread names are of the form "th%d:%s" and include a unique number and the name of the thread-proc.
- `<thread-name>` 是线程的唯一名称。主线程称为“main”。其他线程名称的格式为“th%d:%s”，其中包括唯一编号和线程过程的名称。
- `<event-name>`: is the event name.
- `<event-name>` 是事件名称。
- `<repo-id>`: when present, is a number indicating the repository in use. A `def_repo` event is emitted when a repository is opened. This defines the repo-id and associated worktree. Subsequent repo-specific events will reference this repo-id.Currently, this is always "r1" for the main repository. This field is in anticipation of in-proc submodules in the future.
- `<repo-id>` 在存在时，表示正在使用的存储库的编号。当打开存储库时，会发出`def_repo`事件，这会定义repo-id和相关的工作树。随后的与存储库相关的事件将引用此repo-id。当前，对于主要存储库，此值始终为“r1”。此字段预期用于将来可能出现的in-proc子模块。
- `<t_abs>`: when present, is the absolute time in seconds since the program started.
- `<t_abs>` 在存在时，表示自程序启动以来的绝对时间（以秒为单位）。
- `<t_rel>`: when present, is time in seconds relative to the start of the current region. For a thread-exit event, it is the elapsed time of the thread.
- `<t_rel>` 在存在时，表示相对于当前区域开始时间的时间（以秒为单位）。对于线程退出事件，它表示线程的经过时间。
- `<category>`: is present on region and data events and is used to indicate a broad category, such as "index" or "status".
- `<category>` 在区域和数据事件上存在，并用于指示广泛的类别，例如“index”或“status”。
- `<perf-event-message>`: is a free-form `printf()` message intended for human consumption.
- `<perf-event-message>` 是供人类阅读的自由格式的`printf()`消息。

```
15:33:33.532712 wt-status.c:2310                  | d0 | main                     | region_enter | r1  |  0.126064 |           | status     | label:print
15:33:33.532712 wt-status.c:2331                  | d0 | main                     | region_leave | r1  |  0.127568 |  0.001504 | status     | label:print
```

If `GIT_TRACE2_PERF_BRIEF` or `trace2.perfBrief` is true, the `time`, `file`, and `line` fields are omitted.

​	如果`GIT_TRACE2_PERF_BRIEF`或`trace2.perfBrief`为true，则省略`时间`，`文件名`和`行号`字段。

```
d0 | main                     | region_leave | r1  |  0.011717 |  0.009122 | index      | label:preload
```

The PERF target is intended for interactive performance analysis during development and is quite noisy.

​	PERF目标用于开发中的交互式性能分析，并且输出相当多。

### 事件格式

Each event is a JSON-object containing multiple key/value pairs written as a single line and followed by a LF.

​	每个事件是一个JSON对象，包含多个键/值对，以单行形式写入，并在后面跟随一个LF。

```
'{' <key> ':' <value> [',' <key> ':' <value>]* '}' LF
```

Some key/value pairs are common to all events and some are event-specific.

​	某些键/值对对所有事件都是通用的，而某些键/值对是特定于事件的。

#### 通用的键/值对

The following key/value pairs are common to all events:

​	以下键/值对对所有事件都是通用的：

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

- `"event":<event>`:is the event name.
- `"event":<event>`是事件名称。
- `"sid":<sid>`: is the session-id. This is a unique string to identify the process instance to allow all events emitted by a process to be identified. A session-id is used instead of a PID because PIDs are recycled by the OS. For child git processes, the session-id is prepended with the session-id of the parent git process to allow parent-child relationships to be identified during post-processing.
- `"sid":<sid>`是会话ID。这是一个唯一的字符串，用于标识进程实例，以便可以识别由进程发出的所有事件。会话ID用于替代PID，因为PID由操作系统回收。对于Git的子进程，会话ID会添加到父Git进程的会话ID之前，以便在后期处理期间可以识别父子关系。

- `"thread":<thread>`: is the thread name.
- `"thread":<thread>`是线程名称。
- `"time":<time>`: is the UTC time of the event.
- `"time":<time>`是事件的UTC时间。
- `"file":<filename>`: is source file generating the event.
- `"file":<filename>`是生成事件的源文件。
- `"line":<line-number>`: is the integer source line number generating the event.
- `"line":<line-number>`是生成事件的整数源代码行号。
- `"repo":<repo-id>`:when present, is the integer repo-id as described previously.
- `"repo":<repo-id>`在存在时，是先前描述的整数repo-id。

If `GIT_TRACE2_EVENT_BRIEF` or `trace2.eventBrief` is true, the `file` and `line` fields are omitted from all events and the `time` field is only present on the "start" and "atexit" events.

​	如果`GIT_TRACE2_EVENT_BRIEF`或`trace2.eventBrief`为true，则所有事件中将省略`file`和`line`字段，且`time`字段仅在"start"和"atexit"事件上存在。

#### 事件特定的键/值对

- `"version"`

  This event gives the version of the executable and the EVENT format. It should always be the first event in a trace session. The EVENT format version will be incremented if new event types are added, if existing fields are removed, or if there are significant changes in interpretation of existing events or fields. Smaller changes, such as adding a new field to an existing event, will not require an increment to the EVENT format version.

  此事件提供可执行文件的版本和EVENT格式的版本。它应该始终是跟踪会话中的第一个事件。如果添加了新的事件类型，删除了现有字段，或对现有事件或字段的解释进行了重大更改，EVENT格式版本将递增。较小的更改，例如向现有事件添加新字段，则不需要增加EVENT格式版本。

  ```json
  {
  	"event":"version",
  	...
  	"evt":"3",		       # EVENT format version EVENT格式版本
  	"exe":"2.20.1.155.g426c96fcdb" # git version git版本
  }
  ```

  

- "too_many_files"

  This event is written to the git-trace2-discard sentinel file if there are too many files in the target trace directory (see the trace2.maxFiles config option).

  如果目标跟踪目录中的文件过多（参见trace2.maxFiles配置选项），则将写入此事件到git-trace2-discard哨兵文件中。

  ```json
  {
  	"event":"too_many_files",
  	...
  }
  ```

  

- "start"

  This event contains the complete argv received by main().

  此事件包含由main()收到的完整argv。

  ```json
  {
  	"event":"start",
  	...
  	"t_abs":0.001227, # elapsed time in seconds 经过时间（以秒为单位）
  	"argv":["git","version"]
  }
  ```

  

- "exit"

  This event is emitted when git calls `exit()`.

  当git调用`exit()`时，将发出此事件。

  ```json
  {
  	"event":"exit",
  	...
  	"t_abs":0.001227, # elapsed time in seconds 经过时间（以秒为单位）
  	"code":0	  # exit code 退出码
  }
  ```

  

- "atexit"

  This event is emitted by the Trace2 `atexit` routine during final shutdown. It should be the last event emitted by the process.

  Trace2的`atexit`例程在最终关闭期间发出此事件。这应该是进程发出的最后一个事件。

  (The elapsed time reported here is greater than the time reported in the "exit" event because it runs after all other atexit tasks have completed.)

  （此处报告的经过时间比"exit"事件中报告的时间长，因为它在所有其他atexit任务完成后运行。）

  ```json
  {
  	"event":"atexit",
  	...
  	"t_abs":0.001227, # elapsed time in seconds 经过时间（以秒为单位）
  	"code":0          # exit code 退出码
  }
  ```

  

- "signal"

  This event is emitted when the program is terminated by a user signal. Depending on the platform, the signal event may prevent the "atexit" event from being generated.

  当程序被用户信号终止时，将发出此事件。根据平台的不同，信号事件可能会阻止生成"atexit"事件。

  ```json
  {
  	"event":"signal",
  	...
  	"t_abs":0.001227,  # elapsed time in seconds 经过时间（以秒为单位）
  	"signo":13         # SIGTERM, SIGINT, etc. SIGTERM，SIGINT等。
  }
  ```

  

- "error"

  This event is emitted when one of the `BUG()`, `bug()`, `error()`, `die()`, `warning()`, or `usage()` functions are called.

  当调用`BUG()`、`bug()`、`error()`、`die()`、`warning()`或`usage()`函数之一时，将发出此事件。

  ```json
  {
  	"event":"error",
  	...
  	"msg":"invalid option: --cahced", # formatted error message 格式化的错误消息
  	"fmt":"invalid option: %s"	  # error format string 错误格式字符串
  }
  ```

  The error event may be emitted more than once. The format string allows post-processors to group errors by type without worrying about specific error arguments.

  错误事件可能会被发出多次。格式化字符串允许后处理器按类型分组错误，而不必担心特定的错误参数。

- "cmd_path"

  This event contains the discovered full path of the git executable (on platforms that are configured to resolve it).

  此事件包含Git可执行文件的完整路径（在配置为解析它的平台上）。

  ```json
  {
  	"event":"cmd_path",
  	...
  	"path":"C:/work/gfw/git.exe"
  }
  ```

  

- "cmd_ancestry"

  This event contains the text command name for the parent (and earlier generations of parents) of the current process, in an array ordered from nearest parent to furthest great-grandparent. It may not be implemented on all platforms.

  此事件包含当前进程的父进程（以及更早一代的父进程）的文本命令名称，以数组形式按从最近的父进程到最远的曾祖父进程的顺序排列。它可能未在所有平台上实现。

  ```json
  {
  	"event":"cmd_ancestry",
  	...
  	"ancestry":["bash","tmux: server","systemd"]
  }
  ```

  

- "cmd_name"

  This event contains the command name for this git process and the hierarchy of commands from parent git processes.

  此事件包含此Git进程的命令名称以及从父Git进程继承的命令层次结构。

  ```json
  {
  	"event":"cmd_name",
  	...
  	"name":"pack-objects",
  	"hierarchy":"push/pack-objects"
  }
  ```

  Normally, the "name" field contains the canonical name of the command. When a canonical name is not available, one of these special values are used:

  通常，"name"字段包含命令的规范名称。当无法使用规范名称时，将使用以下特殊值之一：

  ```
  "_query_"            # "git --html-path"
  "_run_dashed_"       # when "git foo" tries to run "git-foo" 当"git foo"尝试运行"git-foo"
  "_run_shell_alias_"  # alias expansion to a shell command alias扩展到shell命令
  "_run_git_alias_"    # alias expansion to a git command alias扩展到Git命令
  "_usage_"            # usage error 使用错误
  "cmd_mode"
  ```

  This event, when present, describes the command variant. This event may be emitted more than once.

  如果存在，此事件描述了命令的变体。此事件可能会被发出多次。

  ```json
  {
  	"event":"cmd_mode",
  	...
  	"name":"branch"
  }
  ```

  The "name" field is an arbitrary string to describe the command mode. For example, checkout can checkout a branch or an individual file. And these variations typically have different performance characteristics that are not comparable.

  "name"字段是一个任意字符串，用于描述命令模式。例如，checkout可以checkout一个分支或一个单独的文件。这些变化通常具有不可比较的不同性能特征。

- "alias"

  This event is present when an alias is expanded.

  当扩展别名时，将出现此事件。

  ```json
  {
  	"event":"alias",
  	...
  	"alias":"l",		 # registered alias 注册的别名
  	"argv":["log","--graph"] # alias expansion 别名扩展
  }
  ```

  

- "child_start"

  This event describes a child process that is about to be spawned.

  此事件描述即将生成的子进程。

  ```json
  {
  	"event":"child_start",
  	...
  	"child_id":2,
  	"child_class":"?",
  	"use_shell":false,
  	"argv":["git","rev-list","--objects","--stdin","--not","--all","--quiet"]
  
  	"hook_name":"<hook_name>"  # present when child_class is "hook" 当child_class为"hook"时出现
  	"cd":"<path>"		   # present when cd is required 当需要cd时出现
  }
  ```

  The "child_id" field can be used to match this child_start with the corresponding child_exit event.

  "child_id"字段可用于将此child_start与相应的child_exit事件匹配。

  The "child_class" field is a rough classification, such as "editor", "pager", "transport/*", and "hook". Unclassified children are classified with "?".

  "child_class"字段是一个粗略的分类，例如"editor"、"pager"、"transport/*"和"hook"。未分类的子进程用"?"分类。

- "child_exit"

  This event is generated after the current process has returned from the `waitpid()` and collected the exit information from the child.

  在当前进程从`waitpid()`返回并从子进程收集了退出信息后，将生成此事件。

  ```json
  {
  	"event":"child_exit",
  	...
  	"child_id":2,
  	"pid":14708,	 # child PID 子进程PID
  	"code":0,	 # child exit-code 子进程退出代码
  	"t_rel":0.110605 # observed run-time of child process 子进程的观察运行时间
  }
  ```

  Note that the session-id of the child process is not available to the current/spawning process, so the child’s PID is reported here as a hint for post-processing. (But it is only a hint because the child process may be a shell script which doesn’t have a session-id.)

  请注意，子进程的会话ID对当前/生成进程不可用，因此此处报告子进程的PID仅作为后处理的提示。（但这仅是一个提示，因为子进程可能是一个没有会话ID的shell脚本。）

  Note that the `t_rel` field contains the observed run time in seconds for the child process (starting before the fork/exec/spawn and stopping after the `waitpid()` and includes OS process creation overhead). So this time will be slightly larger than the atexit time reported by the child process itself.

  请注意，`t_rel`字段包含子进程的观察运行时间，以秒为单位（在fork/exec/spawn之前开始，在`waitpid()`之后停止，并包括操作系统进程创建开销）。因此，这个时间会稍微大于子进程本身报告的atexit时间。

- "child_ready"

  This event is generated after the current process has started a background process and released all handles to it.

  此事件在当前进程启动后台进程并释放所有句柄后生成。

  ```json
  {
  	"event":"child_ready",
  	...
  	"child_id":2,
  	"pid":14708,	 # child PID 子进程PID
  	"ready":"ready", # child ready state 子进程就绪状态
  	"t_rel":0.110605 # observed run-time of child process 子进程的观察运行时间
  }
  ```

  Note that the session-id of the child process is not available to the current/spawning process, so the child’s PID is reported here as a hint for post-processing. (But it is only a hint because the child process may be a shell script which doesn’t have a session-id.)

  请注意，子进程的会话ID对当前/生成进程不可用，因此此处报告子进程的PID仅作为后处理的提示。（但这仅是一个提示，因为子进程可能是一个没有会话ID的shell脚本。）

  This event is generated after the child is started in the background and given a little time to boot up and start working. If the child starts up normally while the parent is still waiting, the "ready" field will have the value "ready". If the child is too slow to start and the parent times out, the field will have the value "timeout". If the child starts but the parent is unable to probe it, the field will have the value "error".

  此事件在子进程在后台启动并给予一点时间进行启动和开始工作后生成。如果子进程在父进程仍在等待的情况下正常启动，"ready"字段的值将为"ready"。如果子进程启动过慢且父进程超时，则字段的值将为"timeout"。如果子进程启动，但父进程无法探测到它，则字段的值将为"error"。

  After the parent process emits this event, it will release all of its handles to the child process and treat the child as a background daemon. So even if the child does eventually finish booting up, the parent will not emit an updated event.

  父进程发出此事件后，将释放对子进程的所有句柄，并将子进程视为后台守护程序。因此，即使子进程最终完成启动，父进程也不会发出更新的事件。

  Note that the `t_rel` field contains the observed run time in seconds when the parent released the child process into the background. The child is assumed to be a long-running daemon process and may outlive the parent process. So the parent’s child event times should not be compared to the child’s atexit times.

  请注意，`t_rel`字段包含父进程将子进程释放到后台时的观察运行时间（以秒为单位）。假定子进程是长时间运行的守护进程，可能会比父进程的子事件时间长。因此，不应将父进程的子事件时间与子进程的atexit时间进行比较。

- "exec"

  This event is generated before git attempts to `exec()` another command rather than starting a child process.

  在Git尝试`exec()`而不是启动子进程时，将生成此事件。

  ```json
  {
  	"event":"exec",
  	...
  	"exec_id":0,
  	"exe":"git",
  	"argv":["foo", "bar"]
  }
  ```

  The "exec_id" field is a command-unique id and is only useful if the `exec()` fails and a corresponding exec_result event is generated.	

  "exec_id"字段是一个唯一于命令的ID，仅在`exec()`失败并生成相应的exec_result事件时有用。

- "exec_result"

  This event is generated if the `exec()` fails and control returns to the current git command.

  如果`exec()`失败并且控制权返回到当前Git命令，则将生成此事件。

  ```json
  {
  	"event":"exec_result",
  	...
  	"exec_id":0,
  	"code":1      # error code (errno) from exec() 来自exec()的错误代码（errno）
  }
  ```

  

- "thread_start"

  This event is generated when a thread is started. It is generated from **within** the new thread’s thread-proc (because it needs to access data in the thread’s thread-local storage).

  此事件在启动线程时生成。它是从**新**线程的线程过程中生成的（因为它需要访问线程的线程本地存储中的数据）。

  ```json
  {
  	"event":"thread_start",
  	...
  	"thread":"th02:preload_thread" # thread name 线程名称
  }
  ```

  

- "thread_exit"

  This event is generated when a thread exits. It is generated from **within** the thread’s thread-proc.

  此事件在线程退出时生成。它是从**新**线程的线程过程中生成的。

  ```json
  {
  	"event":"thread_exit",
  	...
  	"thread":"th02:preload_thread", # thread name 线程名称
  	"t_rel":0.007328                # thread elapsed time 线程经过时间
  }
  ```

  

- "def_param"

  This event is generated to log a global parameter, such as a config setting, command-line flag, or environment variable.

  此事件用于记录全局参数，例如配置设置、命令行标志或环境变量。

  ```json
  {
  	"event":"def_param",
  	...
  	"scope":"global",
  	"param":"core.abbrev",
  	"value":"7"
  }
  ```

  

- "def_repo"

  This event defines a repo-id and associates it with the root of the worktree.

  此事件定义了一个repo-id，并将其与工作树的根目录关联。

  ```json
  {
  	"event":"def_repo",
  	...
  	"repo":1,
  	"worktree":"/Users/jeffhost/work/gfw"
  }
  ```

  As stated earlier, the repo-id is currently always 1, so there will only be one def_repo event. Later, if in-proc submodules are supported, a def_repo event should be emitted for each submodule visited.

  如前所述，repo-id当前始终为1，因此只会有一个def_repo事件。以后，如果支持in-proc子模块，则应为访问的每个子模块发出def_repo事件。

- "region_enter"

  This event is generated when entering a region.

  进入区域时生成此事件。

  ```json
  {
  	"event":"region_enter",
  	...
  	"repo":1,                # optional 可选
  	"nesting":1,             # current region stack depth 当前区域栈深度
  	"category":"index",      # optional 可选
  	"label":"do_read_index", # optional 可选
  	"msg":".git/index"       # optional 可选
  }
  ```

  The `category` field may be used in a future enhancement to do category-based filtering.

  `category`字段可用于未来的增强来执行基于类别的过滤。

  `GIT_TRACE2_EVENT_NESTING` or `trace2.eventNesting` can be used to filter deeply nested regions and data events. It defaults to "2".

  `GIT_TRACE2_EVENT_NESTING`或`trace2.eventNesting`可用于过滤深度嵌套的区域和数据事件。默认值为"2"。

- "region_leave"

  This event is generated when leaving a region.

  离开区域时生成此事件。

  ```json
  {
  	"event":"region_leave",
  	...
  	"repo":1,                # optional 可选
  	"t_rel":0.002876,        # time spent in region in seconds  区域内的时间，以秒为单位
  	"nesting":1,             # region stack depth
  	"category":"index",      # optional 可选
  	"label":"do_read_index", # optional 可选
  	"msg":".git/index"       # optional 可选
  }
  ```

  

- "data"

  This event is generated to log a thread- and region-local key/value pair.

  生成此事件以记录线程和区域本地键/值对。

  ```json
  {
  	"event":"data",
  	...
  	"repo":1,              # optional 可选
  	"t_abs":0.024107,      # absolute elapsed time 绝对经过时间
  	"t_rel":0.001031,      # elapsed time in region/thread 区域/线程内经过时间
  	"nesting":2,           # region stack depth 区域堆栈深度
  	"category":"index",
  	"key":"read/cache_nr",
  	"value":"3552"
  }
  ```

  The "value" field may be an integer or a string.

  "value"字段可以是整数或字符串。

- "data-json"

  This event is generated to log a pre-formatted JSON string containing structured data.

  生成此事件以记录包含结构化数据的预格式化JSON字符串。

  ```json
  {
  	"event":"data_json",
  	...
  	"repo":1,              # optional 可选
  	"t_abs":0.015905,
  	"t_rel":0.015905,
  	"nesting":1,
  	"category":"process",
  	"key":"windows/ancestry",
  	"value":["bash.exe","bash.exe"]
  }
  ```

  

- "th_timer"

  This event logs the amount of time that a stopwatch timer was running in the thread. This event is generated when a thread exits for timers that requested per-thread events.

  此事件记录线程中计时器运行的时间。在线程退出时生成此事件，适用于请求了每线程事件的计时器。

  ```json
  {
  	"event":"th_timer",
  	...
  	"category":"my_category",
  	"name":"my_timer",
  	"intervals":5,         # number of time it was started/stopped 它启动/停止的次数
  	"t_total":0.052741,    # total time in seconds it was running 它运行的总时间（秒）
  	"t_min":0.010061,      # shortest interval 最短时间间隔
  	"t_max":0.011648       # longest interval 最长时间间隔
  }
  ```

  

- "timer"

  This event logs the amount of time that a stopwatch timer was running aggregated across all threads. This event is generated when the process exits.

  此事件记录计时器在所有线程中累计运行的时间。在进程退出时生成此事件。

  ```json
  {
  	"event":"timer",
  	...
  	"category":"my_category",
  	"name":"my_timer",
  	"intervals":5,         # number of time it was started/stopped 它启动/停止的次数
  	"t_total":0.052741,    # total time in seconds it was running 它运行的总时间（秒）
  	"t_min":0.010061,      # shortest interval 最短时间间隔
  	"t_max":0.011648       # longest interval 最长时间间隔
  }
  
  ```

  

- "th_counter"

  This event logs the value of a counter variable in a thread. This event is generated when a thread exits for counters that requested per-thread events.

  此事件记录线程中计数器变量的值。在线程退出时生成此事件，适用于请求了每线程事件的计数器。

  ```json
  {
  	"event":"th_counter",
  	...
  	"category":"my_category",
  	"name":"my_counter",
  	"count":23
  }
  ```

  

- "counter"

  This event logs the value of a counter variable across all threads. This event is generated when the process exits. The total value reported here is the sum across all threads.

  这个事件记录了跨所有线程的一个计数器变量的值。当进程退出时，会生成这个事件。这里报告的总值是所有线程的计数器变量值的总和。

  ```json
  {
  	"event":"counter",
  	...
  	"category":"my_category",
  	"name":"my_counter",
  	"count":23
  }
  ```

## Trace2 API示例用法

Here is a hypothetical usage of the Trace2 API showing the intended usage (without worrying about the actual Git details).

​	以下是Trace2 API的假设用法示例（不考虑实际的Git细节）。

### 初始化

Initialization happens in `main()`. Behind the scenes, an `atexit` and `signal` handler are registered.

​	初始化在`main()`中进行。在幕后，已注册`atexit`和`signal`处理程序。

```c
int main(int argc, const char **argv)
{
	int exit_code;

	trace2_initialize();
	trace2_cmd_start(argv);

	exit_code = cmd_main(argc, argv);

	trace2_cmd_exit(exit_code);

	return exit_code;
}
```

### 命令细节

After the basics are established, additional command information can be sent to Trace2 as it is discovered.

​	在建立了基本设置后，可以将其他命令信息发送到Trace2，因为这些信息被发现。

```c
int cmd_checkout(int argc, const char **argv)
{
	trace2_cmd_name("checkout");
	trace2_cmd_mode("branch");
	trace2_def_repo(the_repository);

	// emit "def_param" messages for "interesting" config settings.
	// 发送"def_param"消息以记录“有趣”的配置设置。
	trace2_cmd_list_config();

	if (do_something())
	    trace2_cmd_error("Path '%s': cannot do something", path);

	return 0;
}
```

### 子进程

Wrap code spawning child processes.

​	包装生成子进程的代码。

```c
void run_child(...)
{
	int child_exit_code;
	struct child_process cmd = CHILD_PROCESS_INIT;
	...
	cmd.trace2_child_class = "editor";

	trace2_child_start(&cmd);
	child_exit_code = spawn_child_and_wait_for_it();
	trace2_child_exit(&cmd, child_exit_code);
}
```

For example, the following fetch command spawned ssh, index-pack, rev-list, and gc. This example also shows that fetch took 5.199 seconds and of that 4.932 was in ssh.

​	例如，以下fetch命令生成了ssh、index-pack、rev-list和gc。此示例还显示fetch花费了5.199秒，其中ssh花费了4.932秒。

```bash
$ export GIT_TRACE2_BRIEF=1
$ export GIT_TRACE2=~/log.normal
$ git fetch origin
...
$ cat ~/log.normal
version 2.20.1.vfs.1.1.47.g534dbe1ad1
start git fetch origin
worktree /Users/jeffhost/work/gfw
cmd_name fetch (fetch)
child_start[0] ssh git@github.com ...
child_start[1] git index-pack ...
... (Trace2 events from child processes omitted)
child_exit[1] pid:14707 code:0 elapsed:0.076353
child_exit[0] pid:14706 code:0 elapsed:4.931869
child_start[2] git rev-list ...
... (Trace2 events from child process omitted)
child_exit[2] pid:14708 code:0 elapsed:0.110605
child_start[3] git gc --auto
... (Trace2 events from child process omitted)
child_exit[3] pid:14709 code:0 elapsed:0.006240
exit elapsed:5.198503 code:0
atexit elapsed:5.198541 code:0
```

When a git process is a (direct or indirect) child of another git process, it inherits Trace2 context information. This allows the child to print the command hierarchy. This example shows gc as child[3] of fetch. When the gc process reports its name as "gc", it also reports the hierarchy as "fetch/gc". (In this example, trace2 messages from the child process is indented for clarity.)

​	当一个git进程是另一个git进程的（直接或间接）子进程时，它会继承Trace2上下文信息。这使得子进程可以打印命令层次结构。该示例显示gc作为fetch的第3个子进程。当gc进程报告其名称为"gc"时，它还报告层次结构为"fetch/gc"。（在此示例中，为了清晰起见，来自子进程的trace2消息进行了缩进。）

```bash
$ export GIT_TRACE2_BRIEF=1
$ export GIT_TRACE2=~/log.normal
$ git fetch origin
...
$ cat ~/log.normal
version 2.20.1.160.g5676107ecd.dirty
start git fetch official
worktree /Users/jeffhost/work/gfw
cmd_name fetch (fetch)
...
child_start[3] git gc --auto
    version 2.20.1.160.g5676107ecd.dirty
    start /Users/jeffhost/work/gfw/git gc --auto
    worktree /Users/jeffhost/work/gfw
    cmd_name gc (fetch/gc)
    exit elapsed:0.001959 code:0
    atexit elapsed:0.001997 code:0
child_exit[3] pid:20303 code:0 elapsed:0.007564
exit elapsed:3.868938 code:0
atexit elapsed:3.868970 code:0
```

### 区域 Regions

Regions can be used to time an interesting section of code.

​	可以使用区域来计时有趣的代码部分。

```c
void wt_status_collect(struct wt_status *s)
{
	trace2_region_enter("status", "worktrees", s->repo);
	wt_status_collect_changes_worktree(s);
	trace2_region_leave("status", "worktrees", s->repo);

	trace2_region_enter("status", "index", s->repo);
	wt_status_collect_changes_index(s);
	trace2_region_leave("status", "index", s->repo);

	trace2_region_enter("status", "untracked", s->repo);
	wt_status_collect_untracked(s);
	trace2_region_leave("status", "untracked", s->repo);
}

void wt_status_print(struct wt_status *s)
{
	trace2_region_enter("status", "print", s->repo);
	switch (s->status_format) {
	    ...
	}
	trace2_region_leave("status", "print", s->repo);
}
```

In this example, scanning for untracked files ran from +0.012568 to +0.027149 (since the process started) and took 0.014581 seconds.

​	在这个示例中，扫描未跟踪的文件的时间从+0.012568到+0.027149（从进程启动到现在）共计0.014581秒。

```bash
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ git status
...

$ cat ~/log.perf
d0 | main                     | version      |     |           |           |            | 2.20.1.160.g5676107ecd.dirty
d0 | main                     | start        |     |  0.001173 |           |            | git status
d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw
d0 | main                     | cmd_name     |     |           |           |            | status (status)
...
d0 | main                     | region_enter | r1  |  0.010988 |           | status     | label:worktrees
d0 | main                     | region_leave | r1  |  0.011236 |  0.000248 | status     | label:worktrees
d0 | main                     | region_enter | r1  |  0.011260 |           | status     | label:index
d0 | main                     | region_leave | r1  |  0.012542 |  0.001282 | status     | label:index
d0 | main                     | region_enter | r1  |  0.012568 |           | status     | label:untracked
d0 | main                     | region_leave | r1  |  0.027149 |  0.014581 | status     | label:untracked
d0 | main                     | region_enter | r1  |  0.027411 |           | status     | label:print
d0 | main                     | region_leave | r1  |  0.028741 |  0.001330 | status     | label:print
d0 | main                     | exit         |     |  0.028778 |           |            | code:0
d0 | main                     | atexit       |     |  0.028809 |           |            | code:0
```

Regions may be nested. This causes messages to be indented in the PERF target, for example. Elapsed times are relative to the start of the corresponding nesting level as expected. For example, if we add region message to:

​	区域可以嵌套。这导致在PERF目标中缩进消息，例如。经过时间是相对于预期的对应嵌套级别的开始时间的。例如，如果我们将区域消息添加到：

```c
static enum path_treatment read_directory_recursive(struct dir_struct *dir,
	struct index_state *istate, const char *base, int baselen,
	struct untracked_cache_dir *untracked, int check_only,
	int stop_at_first_file, const struct pathspec *pathspec)
{
	enum path_treatment state, subdir_state, dir_state = path_none;

	trace2_region_enter_printf("dir", "read_recursive", NULL, "%.*s", baselen, base);
	...
	trace2_region_leave_printf("dir", "read_recursive", NULL, "%.*s", baselen, base);
	return dir_state;
}
```

We can further investigate the time spent scanning for untracked files.

​	我们可以进一步调查花费在扫描未跟踪文件上的时间。

```bash
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ git status
...
$ cat ~/log.perf
d0 | main                     | version      |     |           |           |            | 2.20.1.162.gb4ccea44db.dirty
d0 | main                     | start        |     |  0.001173 |           |            | git status
d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw
d0 | main                     | cmd_name     |     |           |           |            | status (status)
...
d0 | main                     | region_enter | r1  |  0.015047 |           | status     | label:untracked
d0 | main                     | region_enter |     |  0.015132 |           | dir        | ..label:read_recursive
d0 | main                     | region_enter |     |  0.016341 |           | dir        | ....label:read_recursive vcs-svn/
d0 | main                     | region_leave |     |  0.016422 |  0.000081 | dir        | ....label:read_recursive vcs-svn/
d0 | main                     | region_enter |     |  0.016446 |           | dir        | ....label:read_recursive xdiff/
d0 | main                     | region_leave |     |  0.016522 |  0.000076 | dir        | ....label:read_recursive xdiff/
d0 | main                     | region_enter |     |  0.016612 |           | dir        | ....label:read_recursive git-gui/
d0 | main                     | region_enter |     |  0.016698 |           | dir        | ......label:read_recursive git-gui/po/
d0 | main                     | region_enter |     |  0.016810 |           | dir        | ........label:read_recursive git-gui/po/glossary/
d0 | main                     | region_leave |     |  0.016863 |  0.000053 | dir        | ........label:read_recursive git-gui/po/glossary/
...
d0 | main                     | region_enter |     |  0.031876 |           | dir        | ....label:read_recursive builtin/
d0 | main                     | region_leave |     |  0.032270 |  0.000394 | dir        | ....label:read_recursive builtin/
d0 | main                     | region_leave |     |  0.032414 |  0.017282 | dir        | ..label:read_recursive
d0 | main                     | region_leave | r1  |  0.032454 |  0.017407 | status     | label:untracked
...
d0 | main                     | exit         |     |  0.034279 |           |            | code:0
d0 | main                     | atexit       |     |  0.034322 |           |
```

Trace2 regions are similar to the existing trace_performance_enter() and trace_performance_leave() routines, but are thread safe and maintain per-thread stacks of timers.

​	Trace2区域类似于现有的`trace_performance_enter()`和`trace_performance_leave()`例程，但它们是线程安全的，并维护每个线程的计时器栈。

### 数据消息

Data messages added to a region.

​	添加到区域的数据消息。

```c
int read_index_from(struct index_state *istate, const char *path,
	const char *gitdir)
{
	trace2_region_enter_printf("index", "do_read_index", the_repository, "%s", path);

	...

	trace2_data_intmax("index", the_repository, "read/version", istate->version);
	trace2_data_intmax("index", the_repository, "read/cache_nr", istate->cache_nr);

	trace2_region_leave_printf("index", "do_read_index", the_repository, "%s", path);
}
```

This example shows that the index contained 3552 entries.

​	这个示例显示索引包含了3552个条目。

```
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ git status
...
$ cat ~/log.perf
d0 | main                     | version      |     |           |           |            | 2.20.1.156.gf9916ae094.dirty
d0 | main                     | start        |     |  0.001173 |           |            | git status
d0 | main                     | def_repo     | r1  |           |           |            | worktree:/Users/jeffhost/work/gfw
d0 | main                     | cmd_name     |     |           |           |            | status (status)
d0 | main                     | region_enter | r1  |  0.001791 |           | index      | label:do_read_index .git/index
d0 | main                     | data         | r1  |  0.002494 |  0.000703 | index      | ..read/version:2
d0 | main                     | data         | r1  |  0.002520 |  0.000729 | index      | ..read/cache_nr:3552
d0 | main                     | region_leave | r1  |  0.002539 |  0.000748 | index      | label:do_read_index .git/index
...
```

### 线程事件

Thread messages added to a thread-proc.

​	添加到线程过程中的线程消息。

For example, the multi-threaded preload-index code can be instrumented with a region around the thread pool and then per-thread start and exit events within the thread-proc.

​	例如，多线程preload-index代码可以在线程池周围插入区域，然后在线程过程中每个线程的开始和退出事件。

```c
static void *preload_thread(void *_data)
{
	// start the per-thread clock and emit a message.
	// 启动每个线程的时钟并发出消息。
	trace2_thread_start("preload_thread");

	// report which chunk of the array this thread was assigned.
	// 报告该线程被分配的数组块。
	trace2_data_intmax("index", the_repository, "offset", p->offset);
	trace2_data_intmax("index", the_repository, "count", nr);

	do {
	    ...
	} while (--nr > 0);
	...

	// report elapsed time taken by this thread.
	// 报告该线程花费的时间。
	trace2_thread_exit();
	return NULL;
}

void preload_index(struct index_state *index,
	const struct pathspec *pathspec,
	unsigned int refresh_flags)
{
	trace2_region_enter("index", "preload", the_repository);

	for (i = 0; i < threads; i++) {
	    ... /* create thread */ /* 创建线程 */
	}

	for (i = 0; i < threads; i++) {
	    ... /* join thread */ /* 加入线程 */
	}

	trace2_region_leave("index", "preload", the_repository);
}
```

In this example preload_index() was executed by the `main` thread and started the `preload` region. Seven threads, named `th01:preload_thread` through `th07:preload_thread`, were started. Events from each thread are atomically appended to the shared target stream as they occur so they may appear in random order with respect other threads. Finally, the main thread waits for the threads to finish and leaves the region.

​	在这个示例中，preload_index()由`main`线程执行，并开始了`preload`区域。七个线程，命名为`th01:preload_thread`到`th07:preload_thread`，被启动。来自每个线程的事件以原子方式附加到共享目标流中，因此它们可能以随机顺序出现，与其他线程无关。最后，主线程等待线程完成并离开区域。

Data events are tagged with the active thread name. They are used to report the per-thread parameters.

​	数据事件会带有活动线程的名称标签。它们用于报告每个线程的参数。

```bash
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ git status
...
$ cat ~/log.perf
...
d0 | main                     | region_enter | r1  |  0.002595 |           | index      | label:preload
d0 | th01:preload_thread      | thread_start |     |  0.002699 |           |            |
d0 | th02:preload_thread      | thread_start |     |  0.002721 |           |            |
d0 | th01:preload_thread      | data         | r1  |  0.002736 |  0.000037 | index      | offset:0
d0 | th02:preload_thread      | data         | r1  |  0.002751 |  0.000030 | index      | offset:2032
d0 | th03:preload_thread      | thread_start |     |  0.002711 |           |            |
d0 | th06:preload_thread      | thread_start |     |  0.002739 |           |            |
d0 | th01:preload_thread      | data         | r1  |  0.002766 |  0.000067 | index      | count:508
d0 | th06:preload_thread      | data         | r1  |  0.002856 |  0.000117 | index      | offset:2540
d0 | th03:preload_thread      | data         | r1  |  0.002824 |  0.000113 | index      | offset:1016
d0 | th04:preload_thread      | thread_start |     |  0.002710 |           |            |
d0 | th02:preload_thread      | data         | r1  |  0.002779 |  0.000058 | index      | count:508
d0 | th06:preload_thread      | data         | r1  |  0.002966 |  0.000227 | index      | count:508
d0 | th07:preload_thread      | thread_start |     |  0.002741 |           |            |
d0 | th07:preload_thread      | data         | r1  |  0.003017 |  0.000276 | index      | offset:3048
d0 | th05:preload_thread      | thread_start |     |  0.002712 |           |            |
d0 | th05:preload_thread      | data         | r1  |  0.003067 |  0.000355 | index      | offset:1524
d0 | th05:preload_thread      | data         | r1  |  0.003090 |  0.000378 | index      | count:508
d0 | th07:preload_thread      | data         | r1  |  0.003037 |  0.000296 | index      | count:504
d0 | th03:preload_thread      | data         | r1  |  0.002971 |  0.000260 | index      | count:508
d0 | th04:preload_thread      | data         | r1  |  0.002983 |  0.000273 | index      | offset:508
d0 | th04:preload_thread      | data         | r1  |  0.007311 |  0.004601 | index      | count:508
d0 | th05:preload_thread      | thread_exit  |     |  0.008781 |  0.006069 |            |
d0 | th01:preload_thread      | thread_exit  |     |  0.009561 |  0.006862 |            |
d0 | th03:preload_thread      | thread_exit  |     |  0.009742 |  0.007031 |            |
d0 | th06:preload_thread      | thread_exit  |     |  0.009820 |  0.007081 |            |
d0 | th02:preload_thread      | thread_exit  |     |  0.010274 |  0.007553 |            |
d0 | th07:preload_thread      | thread_exit  |     |  0.010477 |  0.007736 |            |
d0 | th04:preload_thread      | thread_exit  |     |  0.011657 |  0.008947 |            |
d0 | main                     | region_leave | r1  |  0.011717 |  0.009122 | index      | label:preload
...
d0 | main                     | exit         |     |  0.029996 |           |            | code:0
d0 | main                     | atexit       |     |  0.030027 |           |            | code:0
```

In this example, the preload region took 0.009122 seconds. The 7 threads took between 0.006069 and 0.008947 seconds to work on their portion of the index. Thread "th01" worked on 508 items at offset 0. Thread "th02" worked on 508 items at offset 2032. Thread "th04" worked on 508 items at offset 508.

​	在这个例子中，`preload` 区域耗时为 0.009122 秒。7个线程的耗时分别为 0.006069 到 0.008947 秒，它们分别处理索引的不同部分。线程 "th01" 处理偏移量为 0 的 508 个项目，线程 "th02" 处理偏移量为 2032 的 508 个项目，线程 "th04" 处理偏移量为 508 的 508 个项目。

This example also shows that thread names are assigned in a racy manner as each thread starts.

​	这个例子还显示了线程名称是在线程启动时被分配的，因此可能存在竞争条件。

### 配置（def param）事件

Dump "interesting" config values to trace2 log.

​	将“有趣的”配置值转储到 trace2 日志。

We can optionally emit configuration events, see `trace2.configparams` in [git-config[1\]](https://git-scm.com/docs/git-config) for how to enable it.

​	我们可以选择发出配置事件，参考 `git-config[1\]` 中的 `trace2.configparams`，了解如何启用它。

```bash
$ git config --system color.ui never
$ git config --global color.ui always
$ git config --local color.ui auto
$ git config --list --show-scope | grep 'color.ui'
system  color.ui=never
global  color.ui=always
local   color.ui=auto
```

Then, mark the config `color.ui` as "interesting" config with `GIT_TRACE2_CONFIG_PARAMS`:

​	然后，将配置 `color.ui` 标记为 "有趣" 的配置项，使用 `GIT_TRACE2_CONFIG_PARAMS`：

```bash
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ export GIT_TRACE2_CONFIG_PARAMS=color.ui
$ git version
...
$ cat ~/log.perf
d0 | main                     | version      |     |           |           |              | ...
d0 | main                     | start        |     |  0.001642 |           |              | /usr/local/bin/git version
d0 | main                     | cmd_name     |     |           |           |              | version (version)
d0 | main                     | def_param    |     |           |           | scope:system | color.ui:never
d0 | main                     | def_param    |     |           |           | scope:global | color.ui:always
d0 | main                     | def_param    |     |           |           | scope:local  | color.ui:auto
d0 | main                     | data         | r0  |  0.002100 |  0.002100 | fsync        | fsync/writeout-only:0
d0 | main                     | data         | r0  |  0.002126 |  0.002126 | fsync        | fsync/hardware-flush:0
d0 | main                     | exit         |     |  0.000470 |           |              | code:0
d0 | main                     | atexit       |     |  0.000477 |           |              | code:0
```

### 计时器事件

Measure the time spent in a function call or span of code that might be called from many places within the code throughout the life of the process.

​	测量在函数调用或代码段中所花费的时间，该代码段可能在整个进程的生命周期内从代码的许多位置调用。

```c
static void expensive_function(void)
{
	trace2_timer_start(TRACE2_TIMER_ID_TEST1);
	...
	sleep_millisec(1000); // Do something expensive
	...
	trace2_timer_stop(TRACE2_TIMER_ID_TEST1);
}

static int ut_100timer(int argc, const char **argv)
{
	...

	expensive_function();

	// Do something else 1...

	expensive_function();

	// Do something else 2...

	expensive_function();

	return 0;
}
```

In this example, we measure the total time spent in `expensive_function()` regardless of when it is called in the overall flow of the program.

​	在这个例子中，我们测量了 `expensive_function()` 调用所花费的总时间，无论它在程序的整体流程中的哪个位置被调用。

```bash
$ export GIT_TRACE2_PERF_BRIEF=1
$ export GIT_TRACE2_PERF=~/log.perf
$ t/helper/test-tool trace2 100timer 3 1000
...
$ cat ~/log.perf
d0 | main                     | version      |     |           |           |              | ...
d0 | main                     | start        |     |  0.001453 |           |              | t/helper/test-tool trace2 100timer 3 1000
d0 | main                     | cmd_name     |     |           |           |              | trace2 (trace2)
d0 | main                     | exit         |     |  3.003667 |           |              | code:0
d0 | main                     | timer        |     |           |           | test         | name:test1 intervals:3 total:3.001686 min:1.000254 max:1.000929
d0 | main                     | atexit       |     |  3.003796 |          
```

## 未来工作

### 与现有的 Trace Api (api-trace.txt) 的关系

There are a few issues to resolve before we can completely switch to Trace2.

​	在完全切换到 Trace2 之前，有一些问题需要解决：

- Updating existing tests that assume `GIT_TRACE` format messages.
- 更新现有测试，这些测试假定 `GIT_TRACE` 格式的消息。
- How to best handle custom `GIT_TRACE_<key>` messages?
- 如何处理自定义的 `GIT_TRACE_<key>` 消息？
  - The `GIT_TRACE_<key>` mechanism allows each <key> to write to a different file (in addition to just stderr).
  - `GIT_TRACE_<key>` 机制允许每个 `<key>` 将消息写入不同的文件（除了 stderr）。
  - Do we want to maintain that ability or simply write to the existing Trace2 targets (and convert <key> to a "category").
  - 我们是否要保留这种能力，还是直接写入现有的 Trace2 目标（并将 `<key>` 转换为 "category"）？













