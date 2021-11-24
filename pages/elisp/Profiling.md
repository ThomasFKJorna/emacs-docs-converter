

### 18.5 Profiling

If your program is working correctly, but not fast enough, and you want to make it run more quickly or efficiently, the first thing to do is *profile* your code so that you know where it spends most of the execution time. If you find that one particular function is responsible for a significant portion of the execution time, you can start looking for ways to optimize that piece.

Emacs has built-in support for this. To begin profiling, type `M-x profiler-start`. You can choose to profile by processor usage, memory usage, or both. Then run the code you’d like to speed up. After that, type `M-x profiler-report` to display a summary buffer for each resource (cpu and memory) that you chose to profile. The names of the report buffers include the times at which the reports were generated, so you can generate another report later on without erasing previous results. When you have finished profiling, type `M-x profiler-stop` (there is a small overhead associated with profiling, so we don’t recommend leaving it active except when you are actually running the code you want to examine).

The profiler report buffer shows, on each line, a function that was called, followed by how much resources (cpu or memory) it used in absolute and percentage terms since profiling started. If a given line has a ‘`+`’ symbol at the left-hand side, you can expand that line by typing `RET`, in order to see the function(s) called by the higher-level function. Use a prefix argument (`C-u RET`) to see the whole call tree below a function. Pressing `RET` again will collapse back to the original state.

Press `j` or `mouse-2` to jump to the definition of a function at point. Press `d` to view a function’s documentation. You can save a profile to a file using `C-x C-w`. You can compare two profiles using `=`.

The `elp` library offers an alternative approach, which is useful when you know in advance which Lisp function(s) you want to profile. Using that library, you begin by setting `elp-function-list` to the list of function symbols—those are the functions you want to profile. Then type `M-x elp-instrument-list RET nil RET` to arrange for profiling those functions. After running the code you want to profile, invoke `M-x elp-results` to display the current results. See the file `elp.el` for more detailed instructions. This approach is limited to profiling functions written in Lisp, it cannot profile Emacs primitives.

You can measure the time it takes to evaluate individual Emacs Lisp forms using the `benchmark` library. See the macros `benchmark-run`, `benchmark-run-compiled` and `benchmark-progn` in `benchmark.el`. You can also use the `benchmark` command for timing forms interactively.

To profile Emacs at the level of its C code, you can build it using the `--enable-profiling` option of `configure`. When Emacs exits, it generates a file `gmon.out` that you can examine using the `gprof` utility. This feature is mainly useful for debugging Emacs. It actually stops the Lisp-level `M-x profiler-…` commands described above from working.

***
