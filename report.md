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

### Discussion regarding refactoring
The refactoring is safe and non-invasive in a broader sense in relation to the code base. Nevertheless, there are some key points that are of interest.

#### Benefits
The refactoring was issued by an active author of the code base. There was a clear purpose of the refactoring and it thus had some Benefits.

-The code is shorter and somewhat combats code duplication. The two functions were similar and one could easily see why one would want to merge them.

-Furthermore, it is easier to implement good code coverage as you only need to test one function in place of two. However, this might not be a benefit as the merged function might be of greater complexity.

-The code is easier to maintain and update as possible future changes only need to be built with respect to one function.

-There is a very slight performance increase as the data is handled in a single pass instead of two passes. In addition, the two passes of the old function also contained some filtering of the lists which the new function do not have.
#### Drawbacks
-The code is more complex. Not necessarily actual complexity but as for a human reader, it would be harder to understand. Some functionality is not very intuitive. One can say that the new return value is two return values baked into one. Then the code has to pick the one it actually wants. This might confuse future developers and the new documentation has to take this into effect.

#### Summary
The refactoring is a welcome change to the code. The benefits outweigh the drawbacks in a way that is acceptable. The added complexity of the code is the main problem. Having that added layer of complexity in a return value is not very good and might impact the development process. This can be relatively easy fixed by sound documentation that is clear and concise.
All in all the changes made is a valid solution to the refactoring issue and do not pose any great problems.


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
