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

The class `TaskExecutor.java` is directly tested by only one test class called `TaskExecutorTest.java`. Most of the test cases in this class are made up by Mock-Testing code. The class also has a a method called `setUp` --- with the `@Before` annotation --- , which initializes fields for the following test cases. Moreover, there also exists a method called `checkEqualElements`, which is used to test whether two lists contain the same elements.

In the following text, all test methods and their functionality are listed:

* `testSourceVertexDataFetching` creates a source vertex TODO
* `testUnboundedSourceVertexDataFetching` TOOD
* `testParentTaskDataFetching` TODO


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
