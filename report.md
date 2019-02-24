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

## The refactoring carried out

(Link to) a UML diagram and its description

## Relation to design and refactoring patterns

As described before, our refactoring consists in merging the `getInternalAdditionalOutputMap()` and the `getInternalMainOutputs()` function to a single function, so that the return value is always `Map<String, List<NextIntraTaskOperatorInfo>>` (like in the case of `getInternalAdditionalOutputMap()` currently) and not simply `List<NextIntraTaskOperatorInfo>` (like in the case of `getInternalMainOutputs()` currently). In `getInternalAdditionalOutputMap()`, all outgoing edges that have the same so-called output tag will be mapped from that output tag to a list of `NextIntraTaskOperatorInfo` objects, which is a list containing for every edge the edge index, the destination of the edge (i.e. a vertex) and the watermark of that vertex. In `getInternalMainOutputs()`, `NextIntraTaskOperatorInfo` objects are created from all outgoing edges that don't have any output tags. We will merge this behaviour, so that if there is no output tag for an outgoing edge, we will map all `NextIntraTaskOperatorInfo` objects of those edges from a null string.

We wouldn't say that this deviates very much from the overall software architecture and design patterns of the project, since we've simply merged the behaviour of two similar functions. One difference now is that in the `prepare` function, from where our refactored function will be called, you will have to extract the `internalMainOutputs` list from the output map of the function by getting the list mapped from the null string. One design pattern deviation might be that we will be using an if statement instead of a stream filtering condition to check if the edges have output tags, but this is essentially the same thing.

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
