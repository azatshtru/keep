stack is a todo.txt based task management system, mostly a parser for commandline. Main tasks will be handled through ryth. Maybe this can upload them to ryth in some way? who knows.

pmrs is an asynchronous pomodoro timer written in rust for the commandline, it runs on a simple async_server available on local network.
# pmrs
- [ ] finish studying async rust and implement the async_server in it.
- [ ] make notes about async rust.
- [ ] implement async timer
- [ ] implement async stdin
- [ ] implement tasks integration through stack
- [ ] if tasks are completed between a timer, then the timer will store it in some data structure for analysis later, so that tasks that were completed during a pomodoro can be tracked.
- [ ] make it beautiful

# stack
- [ ] implement simple AST 
- [ ] implement a simple datastructure to parse the AST into cohesive tasks
- [ ] implement API functions to add, remove, prioritize, assign due dates, etc. to tasks.