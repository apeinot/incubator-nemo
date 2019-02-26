# Report for assignment 4

This is a template for your report. You are free to modify it as needed.
It is not required to use markdown for your report either, but the report
has to be delivered in a standard, cross-platform format.

## Project

Name: Apache Nemo

URL: https://github.com/apache/incubator-nemo

Apache Nemo is a data processing system currently under development.

## Architectural overview (optional, as one item for P+)

## Selected issue(s)

Title: Refactor getInternal(Main/Additional)OutputMap in TaskExecutor

URL: https://issues.apache.org/jira/browse/NEMO-253

There are two separate methods to get main or additional output maps in TaskExecutor. The refactor task consist to merge the both beacause they share a lot of code.

## Onboarding experience

Everything build as documented if we respect all the prerequisities.
    

## Requirements affected by functionality being refactored

The function to be refactored ("getInternalMainOutputs" and "getInternalAdditionalOutput") are part of the [TaskExecutor class](https://github.com/apeinot/incubator-nemo/blob/lab4/runtime/executor/src/main/java/org/apache/nemo/runtime/executor/task/TaskExecutor.java). They are called in the "prepare" method which is called in the constructor of the class. All these functions have to be taken into account to plan the refactoring. The following tables describe the requirements of the class TaskExecutor and the relevant functions.

| Identifier                      | TaskExecutor                                                                                                                                                           |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Title                           | Executing a task                                                                                                                                                       |
| Requirement Description         | Dispose from an object that can contain and execute a given task                                                                                                       |
| Validity checks on inputs       | The constructor from the class shall only accept input that matches the type given in the constructor declaration                                                                |
| Sequence of Operations          | A TaskExecutor object shall contain all the needed information on a task and be able to execute it when asked                                                          |
| Response to abnormal situations | The class shall throw exceptions if an unexpected behaviour or use is detected                                                                                         |
| Effect of Parameters            | The attributes from the class shall contain all the info to execute a task                                                                                             |
| Relationship of output to input | To be created, a TaskExecutor needs all the info related to a task. The TaskExecutor shall be able to return the information related to the execution of the given task |
| Priority                        | Very high                                                                                                                                                              |
| Verifiability                   | A TaskExecutor object should be instantiated with the correct inputs and run the corresponding task when asked                                                           |
| Dependency                      |                                                                                                                                                                        |

| Identifier                      | constructor                                                                                                                           |
|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Title                           | Building a TaskExecutor                                                                                                               |
| Requirement Description         | A constructor which creates a new TaskExecutor and sets instance variables. A TaskExecutor is an object that can execute a given task |
| Validity checks on inputs       | The constructor shall only accept input that matches the type given in the constructor declaration                                                     |
| Sequence of Operations          | The constructor shall set all instance variables to the variables given as input, then invoke the prepare method                      |
| Response to abnormal situations | The method shall throw an exception upon encountering abnormal behavior                                                               |
| Effect of Parameters            | The parameters shall specify the task to be executed as well as the instance variables of the class                                   |
| Relationship of output to input | The input defines the parameters of the class. There are no real output of the method except the new object itself.                     |
| Priority                        | High                                                                                                                                  |
| Verifiability                   | After creation, all instance variables are set according to the input when creating the class, and the prepare method is called       |
| Dependency                      | TaskExecutor                                                                                                                            |


| Identifier                      | prepare                                                                                                                                                                                                                               |
|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Title                           | Preparing the task for execution                                                                                                                                                                                                      |
| Requirement Description         | A method which can set up the necessary input variables required to fetch internal outputs and handles the main control flow. At the end of the execution, the task contained in the TaskExecutor should be ready to execute.           |
| Validity checks on inputs       | The function shall only accept input that matches the type given in the method handle                                                                                                                                                 |
| Sequence of Operations          | The method shall first create a map mapping edges to indexes and a map mapping IRVertex to InputWatermarkManager, after which it shall invoke the getInternal methods and set up the relevant variable for the task and sub-tasks |
| Response to abnormal situations | The method shall throw an exception upon encountering abnormal behavior                                                                                                                                                               |
| Effect of Parameters            | The parameters shall specify the task, a DAG and an IntermediateDataIOFactory used for communication with other tasks                                                                                                                 |
| Relationship of output to input | The method shall return fetchers and harnesses given by the task to be executed                                                                                                                                                       |
| Priority                        | High                                                                                                                                                                                                                                  |
| Verifiability                   | During its execution, the method creates a map mapping edges to indexes, and a map mapping IRVertex to InputWatermarkManager and invokes both getInternal methods                                                                     |
| Dependency                      | constructor                                                                                                                                                                                                                           |

| Identifier                      | getInternalMain                                                                                                                                                                                                   |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Title                           | Fetching main Intra Tasks info                                                                                                                                                                                    |
| Requirement Description         | The method can identify edges without output tag and fetch the Next Intra Task Operator Info                                                                                                                      |
| Validity checks on inputs       | The method shall only accept input that matches the type given in the method handle                                                                                                                               |
| Sequence of Operations          | The method shall filter the given DAG by edges without an output tag, after which it shall store (create a list of) the NITOI and return them                                                                     |
| Response to abnormal situations | The method shall throw an exception upon encountering abnormal behavior                                                                                                                                           |
| Effect of Parameters            | The parameters shall specify from which vertex the outgoing edges shall be checked, a DAG containing all vertices and edges, a map mapping edges to indices and a map mapping IRVertices to InputWatermarkManager |
| Relationship of output to input | The method shall use the given map and IWM to create a list of NITOI, which are created from the index and destination of edges without output tag as well as the IWM                                             |
| Priority                        | High                                                                                                                                                                                                              |
| Verifiability                   | Method returns a list of the NITOI for all edges without output tag                                                                                                                                             |
| Dependency                      | prepare                                                                                                                                                                                                           |


| Identifier                      | getInternalAdditional                                                                                                                                                                                             |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Title                           | Fetching additional Intra Tasks info                                                                                                                                                                              |
| Requirement Description         | The method can identify edges with output tag and fetches their Next Intra Task Operator Info                                                                                                                       |
| Validity checks on inputs       | The method shall only accept input that matches the type given in the method handle                                                                                                                               |
| Sequence of Operations          | The method shall filter the given DAG by edges with an output tag, then map the output tags to a list of NITOI                                                                                                    |
| Response to abnormal situations | The method shall throw an exception upon encountering abnormal behavior                                                                                                                                           |
| Effect of Parameters            | The parameters shall specify from which vertex the outgoing edges shall be checked, a DAG containing all vertices and edges, a map mapping edges to indices and a map mapping IRVertices to InputWatermarkManager |
| Relationship of output to input | The method shall use the given map and IWM to create a mapping of edge output tags to NITOI, which are created from the index and destination of an edge as well as the IWM                                       |
| Priority                        | High                                                                                                                                                                                                              |
| Verifiability                   | Method returns a map mapping output tag to list of NITOI                                                                                                                                                          |
| Dependency                      | prepare                                                                                                                                                                                                           |

The above requirements have been deduced from the code, its documentation and the existing test cases (see next section for more details about tests). Our refactoring will correspond to fullfil the requirements *getInternalMain* and *getInternalAdditional* with only one method against two in the current version of the code.

## Existing test cases relating to refactored code

## The refactoring carried out

(Link to) a UML diagram and its description

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
