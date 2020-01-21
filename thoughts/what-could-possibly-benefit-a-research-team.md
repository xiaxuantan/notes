I feel extremely deflated and fatigued at the moment. Clearly, four hours of sleep is not enough for me to recover from staying up late. Last night I helped a research team with some experiments and worked until 3. Though, the research team failed to meet the deadline and decide to make it to the next conference.

I am not a member of this team but it's heartbreaking to see their effort squandered. A lot of things did not go well in the process and many were from the algorithm implementation from my perspective. Frankly, I had very few experiences working as a researcher but as a software engineer who suffered from shitty code in the past few years, I definitely know how difficult it is to get a poorly implemented running.

Thus, I find it necessary to document things that might help to improve the code quality. Following are some of my thoughts. 

- Maintain a good codebase

  Writing code beautifully is easier said than done. Some of their legacy code really makes me feel uncomfortable. A long method, large class, and bad naming make the code really unreadable. I think a good codebase should meat at least (but not limited to) these requirements.

  - Follow the official style guide. For example, read PEP8 carefully if Python3 is used.
  - Add docstring (documentation) for all external internal and external interfaces.
  - Try to avoid nested conditions. Multiple levels of indentation make readers feel like they are lost in a maze.

  There is also a lot more to mention because writing readable code is a huge topic. I would encourage each member in a research team, no matter a coder or a paper writer, to inspect the codebase carefully.

- Unit tests

  Disappointedly, very few researchers write or consider writing, unit tests for their functions. They tend to validate the results from the uppermost interface, which makes it really difficult to debug. By adding unit tests, the coder can redesign the code structure and decompose its complex segments into simple and testable pieces. Thus, find a corresponding recipe for your programing language to run unit tests automatically.

- Do profiling

  A lot of beginners love to wrap the function call with start/end timestamps in order to calculate the time spent on this function. Here I recommend using a more professional tool to do that. For example, I use PyCharm profiling for my python scripts. It generates a call graph that gives detailed information including invocation counts and total costs for each function. There must be a wide range of choices so it's ok to pick up any of them.

  Do profiling in extreme cases as early as possible. Only in such scenarios, the bottleneck could be detected.

- Setting up environments

  - Scripts. What could be worse than spending hours to set up an environment with all libraries, binaries and configuration files? The answer is setting up such environment on another machine all over again. It is very important to write down the steps taken and the pits encountered.
  - Versioning. Record the release number, tag or commit ID for every dependency. This is true when you are installing software. This is also true when you expose interfaces to others.
  - The possibility of using Docker. Sometimes, the upgrade of host OS results in the incompatibility of some software. It sounds a good idea to bundle all software, including OS, into a Docker image so that it can run across all platforms. A potential problem is that computing-intensive tasks run much slower in containers than native processes from my observation. More investigation is needed.

- Pipelining

  Usually, the numeric results and metrics are collected and drawn. Therefore, scripts and tools are needed to automate the whole process. 

  - Invisible Overheads. Be really careful with the overhead caused by the high-level wrappers. When you are calculating the time spent on a task, run it in a lightweight environment and precisely calculate only the time spent on a particular function.
  - Dataset and results naming convention. Do include all variables in the file path of input and output.
  - Timeout. Sometimes we are uncertain if a certain program is capable of generating results in a given time so we would measure it by feeding a list of arguments, from simple to complicated. It is good to stop if timeout is detected.

- Optimization

  When you found that the code runs slower than expected, there are some directions 

  - Preprocessing. If there are some immutable yet repeatedly computed results,  try to save them beforehand.
  - Caching. For some computing-intensive tasks with a very simple argument list, it is always to good to consider using cache. Calculating the hit rate is a good method to evaluate improvement.
  - Using simpler data structures. Try to use arrays over set/map; try to use a primitive type key when using a key-value store.
  - Parallel.

- More to be added......

Lastly, I want to say that for a research team, it is also important to manage code just like a software engineer does. While I am amazed at their innovative and insightful thoughts, I really hope that by treating software more seriously and professionally, they could achieve greater success.