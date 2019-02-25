# Report for assignment 4

This is a template for your report. You are free to modify it as needed.
It is not required to use markdown for your report either, but the report
has to be delivered in a standard, cross-platform format.

## Project

Name: Apache Nemo

URL: https://github.com/apache/incubator-nemo

Apache Nemo is a data processing system currently under development.
## Nemo overview
### Purpose
The purpose of Nemo is to increase the performance and efficiency of an application via flexible control of its runtime behaviour. By adapting the runtime behaviour it can be adapted to fit any characteristics and/or situation, which leads to greater efficiency.

### General Architecture
The data processing of Nemo is done in two steps, compilation and runtime. While runtime is more or less what one might expect, the compilation is somewhat less intuitive. First, the compiler will be explained in a larger scope followed by runtime. Then, deeper explanation regarding key concepts and structures.

#### Compiler
In order for Nemo to increase the efficiency and optimize runtime, it first has to come up with a plan. While it's easy to assume that the compiler compiles code in a traditional sense, this is actually not the purpose of the compiler.
The compiler produces what is know as Nemo Intermediate Representation or 'IR' which is, in essence, a description of the *order of operations* of runtime. More on the IR later.
The compiler works in three stages. The frontend, the optimization and the backend.

##### Frontend
The frontend is what works directly against the process being optimized. It is here that the Nemo intermediate Representation is first created. The frontend translates high-level data-flow programming languages into IR. The frontend traverses the dataflow logic in topological order and appends their IR counterpart to an IR builder. After checking integrity, the IR is built.

##### Optimizer
The optimizer uses what is know as a _Nemo policy_ to optimize the flow within the IR. It is here where the true function of Nemo lays. The optimization is done in several _passes_. A policy might have several optimizations provided in a critical order. Each optimization requires a separate pass over the IR.

##### Backend
The backend finalizes the plan for execution. The IR is traversed and translated into a _physical execution plan_. The IR vertices (operations) are grouped based on stage numbers. In this manner, all vertices with the same stage number can be concurrently performed to increase performance. These operations are hereon denoted as 'tasks'. The backend passes the tasks, stage group and some additional data dependency information to the runtime execution. This physical plan can be seen as a directed acyclic graph (DAG).

#### Runtime
Now Nemo is ready to execute the _Physical execution plan_. In order to cope with the parallelism, Nemo uses a primary/master runtime 'master' as well as several secondary/slave 'executors'.
The master keeps track of the execution plan while the executor performs the stages. Apart from just managing when to execute what, the master and the executors also share some data storage. This storage is actively managed by the RuntimeMaster. So when tasks need to share information it is done through 'Blocks' in a common storage. Special modules within the master and executors handle this correspondence.

### Optimization staples

#### Optimization pass
An optimization pass is one pass over the IR where a specific optimization is being applied.

#### Nemo Policies
A collection of complete optimization passes is called a _Nemo Policy_. This policy might be designed to have a very specific impact on the runtime. The effects of a policy can be very diverse. The diverse and modular nature of the policies is what gives Nemo its great flexibility.

#### Nemo Intermediate Representation
Nemo Intermediate Representation, IR, is an abstraction of a data format used to describe the data flow of an application. The IR can be seen as a directed acyclic graph, DAG, and consists of vertices and edges. There are different kinds of vertices for different actions such as logical operation and input/output. It is this standardized format that allows for efficient optimization of dataflow.

### Summary
Nemo works in two stages with additional sub-stages
- Compilation:

    - Translation of data flow to IR

    - Optimization of IR

    - Translate IR to the execution plan

- Runtime execution

    - A master keeps track of the state of the execution plan and all tasks.

    - Several executors perform atomized tasks or groups of tasks.


## Selected issue(s)

Title: Refactor getInternal(Main/Additional)OutputMap in TaskExecutor

URL: https://issues.apache.org/jira/browse/NEMO-253

There are two separate methods to get main or additional output maps in TaskExecutor. The refactor task consist to merge the both beacause they share a lot of code.

## Onboarding experience

Everything build as documented if we respect all the prerequisities.


## Requirements affected by functionality being refactored

## Existing test cases relating to refactored code

## The refactoring carried out

(Link to) a UML diagram and its description

## Relation to design and refactoring patterns

As described before, our refactoring consists in merging the `getInternalAdditionalOutputMap()` and the `getInternalMainOutputs()` function to a single function. This way, the return value is always `Map<String, List<NextIntraTaskOperatorInfo>>` (like in the case of `getInternalAdditionalOutputMap()` currently) and not simply `List<NextIntraTaskOperatorInfo>` (like in the case of `getInternalMainOutputs()` currently). In `getInternalAdditionalOutputMap()`, all outgoing edges that have the same so-called output tag will be mapped from that output tag to a list of `NextIntraTaskOperatorInfo` objects, which is a list containing for every edge the edge index, the destination of the edge (i.e. a vertex) and the watermark of that vertex. In `getInternalMainOutputs()`, `NextIntraTaskOperatorInfo` objects are created from all outgoing edges that don't have any output tags. We will merge this behaviour, so that if there is no output tag for an outgoing edge, we will map all `NextIntraTaskOperatorInfo` objects of those edges from a null string.

This doesn't deviate very much from the overall software architecture and design patterns of the project, since the difference is simply that the behaviour of two similar functions has been merged. Another difference is that in the `prepare` function, from where our refactored function will be called, you will have to extract the `internalMainOutputs` list from the output map of the function by getting the list mapped from the null string. One design pattern deviation might be that an if statement will be used instead of a stream filtering condition to check if the edges have output tags, but this is essentially the same thing.

## Test logs

Overall results with link to a copy of the logs (before/after refactoring).

The refactoring itself is documented by the git log.

## Effort spent

For each team member, how much time was spent in

1. plenary discussions/meetings;

2. discussions within parts of the group;

3. reading documentation;

4. configuration;

5. analyzing code/output;

6. writing documentation;

7. writing code;

8. running code?

## Overall experience

What are your main take-aways from this project? What did you learn?

Is there something special you want to mention here?
