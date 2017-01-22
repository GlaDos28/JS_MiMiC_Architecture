# JS MiMiC Architecture

JavaScript *Module-Mediator-Controller* architecture (or *MiMiC*, *MMC*) is a way to create JS back-end programs in a specific ellegant style.

### How it works on paper?

As realized by architecture name, it separates the program on three logical parts.

1. ***Module*** (or ***TODO***) - an architecture unit, which implements the concrete task, formally named *executive task*. These and only these units form all the technical logic part of the program. Moreover, according to the architecture, modules are the only objects that changes between different programs, so the programmer needs to use only them to create his project - other code part is crearly defined and automated.

2. ***Mediator*** (or ***Core***) - an architecture unit, which implements the execution of tasks handled by modules. Cores can be compared with processors in a computer that manages tasks and their completion. Mediators have their own task queue. These elements finalize the technical task part.

3. ***Control unit*** (***CU***, or ***Controller***) - an architecture unit, which implements the cores' manager system. It is also the communicator between business logic and technical implementation. The controller is only one for the project, against the potentially unlimited number of mediatros and modules. As it is the input point for program users, controller accepts human-written tasks, formally named *business tasks*. It consists of under-controlled core collection for executing the given tasks and mapping { business task : executive tasks }. This element finalizes the business task part.

Now we describe the operating cycle of a program based on MiMiC architecture.

* Firstly, any user requests a business task to be handled. The task with necessary arguments is served in the program's Control Unit. The last one knows about the connection of business tasks and special structures that describe business tasks technically, basing on executive tasks (formally the structue named *task package*). The CU selects appropriate mediator to execute the business task and gives to it respective task package with already hardcoded business arguments into the executive tasks.

* Secondly, mediator to be chosen receives task package and put all executives tasks into the queue. As the time comes, executive tasks begin to be handled in order they are described. Tasks is able to share their output as argument to each other, within one task package.

* The execution of a single execcutive task is followed by several notices:
 - Core is searching for mapping { executive task : module } to find the module for completing the task.
 - Module receives arguments that described for it. Argument can be hardcoded from CU or during the output return of another task.
 - During code execution, module is able to request some data without itself, by calling another executive task with arguments. Note that it is the only way to get something from outside the module code, *libraries direct using is denied*.
 - When module requests an outsource data, its execution is frozed until the data be got. Called executive task is put in task queue.
 - When completed, module can return only one result (or error), meaning that it can not be separated on two different datas as arguments.

* When all of the tasks with taskpackage are completed, the final result (provided by the last task) is returned to the CU, which in turn will be sent to the user.
