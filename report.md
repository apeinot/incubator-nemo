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

## Existing test cases relating to refactored code

The class `TaskExecutor.java` is directly tested by only one test class called `TaskExecutorTest.java`. Most of the test cases in this class are made up by Mock-Testing code. Mocking is mostly applied for edges. The class also has a a method called `setUp` --- with the `@Before` annotation --- , which initializes fields for the following test cases. Moreover, there also exist some helper-methods,  which are used for several tasks, e.g., checking whether two Lists have the same elements. Furthermore, `TaskExecutorTest` provides some predefined classes for special test cases, e.g., the class `TestUnboundedSourceVertex` for the test function `testUnboundedSourceVertexDataFetching`.

In the following text, all test methods and their functionality are enlisted:

* `testSourceVertexDataFetching` creates a source vertex and tests whether the data are correctly fetched originating from the source vertex.
* `testUnboundedSourceVertexDataFetching` creates two vertices and tests whether the data are correctly output and whether the watermark is emitted.
* `testParentTaskDataFetching` creates a vertex and tests whether the data for the parent vertex are correctly emitted. This test function has time limit of 5 seconds.
* `testMultipleIncomingEdges` creates in total five vertices. There are two source vertices and three normal vertices. Each source vertex has one normal successor vertex and those two normal verices culminate in another normal vertex. Therefore, the last vertex has two incoming edges. The test function tests whether all created watermarks are emitted in the correct order and whether the ouput is emitted correctly.
* `testTwoOperators` creates two vertices with the same operator that are executed sequentially. It is tested whether the results of the first vertex correctly go to the second vertex. This test function has time limit of 5 seconds.
* `testTwoOperatorsWithBroadcastVariable` is in many points similar to the test function `testTwoOperators`. The big difference is that in this test function the second vertex has a differnt operator than the first vertex. Once again, it is tested whether the test results are correctly emitted according to the operators.
* `testAdditionalOutputs` creates four vertices in total. There is one parent vertex and three child vertices of the parent vertex. Two of the child vertices get an additional tag and both additional tags differ. The test function evaluates whether the output for each of the three child vertices is emitted correctly. The vertices with the additional tags need to have one output more than the child vertex without additional tag. This test function has time limit of 5 seconds.

Each of these seven test methods calls the function `getTaskExecutor`, which creates a new object of the class `TaskExecutor`. By analyzing the constructor of the class `TaskExecutor`, one can clearly see that a method called `prepare` gets executed every time the constructor is called. Again, the method `prepare` calls both functions we affected with our refactoring. This means that the code we refactored is tested in all seven available test cases.


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
