# Test Design

The OpenKCC project is a library intended for a variety of use cases.
To manage new code being added to the project and ensure it does not break
existing features, there are some tests added to the project.

The tests for the project are found in the folder

```text
Packages\com.nickmaltbie.openkcc\Tests
```

with two sub folders, `EditMode` and `PlayMode` for different types of
tests. `EditMode` tests are run as unit tests in the editor while `PlayMode`
tests are meant to be integration tests run in the editor or for a
compiled version of the project. See [Unity Test Framework](https://docs.unity3d.com/Packages/com.unity.test-framework@2.0/manual/index.html)
documentation for more details on these types of tests.

## Running the Tests

To execute the tests, use the `Test Runner` window as part of the
[Unity Test Framework](https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/index.html)
library.

They can be executed from CLI or as part of the test runner under
the `Test Runner` window (See [Workflow: How to run a test](https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/workflow-run-test.html)).

The tests are also run as part of a GitHub action as part of every
PR and with every build of the project.

## Goals

There are few main goals for adding the tests to the project.

1. Ensure that existing features do not break for the project.
1. Verify code works as expected in an automated fashion.
1. Provide examples on how the code can be used.

These goals will help ensure that the OpenKCC project is generally usable
without causing many breaking changes and show other users how
to integrate the project and features into their own work.

## Writing Tests

Each of the different types of test modes has different ways about writing
tests. EditMode tests are unit test simply meant to show that the code can
execute and act as expected. Called unit tests because they evaluate
each part of the code as it's own unit. PlayMode tests are integration tests
meant to demonstrate that different parts (or units) of the code interact
properly together to achieve some more complex function.

### EditMode Tests

Edit mode tests usually end up 'Mocking' other components to make them
easier to test independently (See [Mocking in Unit Tests](https://microsoft.github.io/code-with-engineering-playbook/automated-testing/unit-testing/mocking/)
for a more detailed overview of mocking).

This is achieved using the [Moq](https://github.com/Moq/moq4) library. Please
see some existing examples of edit mode tests for examples.

```text
Packages\com.nickmaltbie.openkcc\Tests\EditMode
```

EditMode tests should provide full coverage of all different code paths to
ensure they execute as expected.

### PlayMode Tests

Play mode tests are different as they are integration tests.
As part of a play mode test, instead of mocking a scene, a real scene will
be created with functioning components and simulated for a number
of updates (usually with time modified to make them much faster) under
different conditions.

```text
Packages\com.nickmaltbie.openkcc\Tests\PlayMode
```

PlayMode tests don't need to provide full coverage, but should instead
be testing individual scenarios of how the project could be used.

The benefit of creating the scene via script instead of as a unity scene is that
it allows for very complex or programmatic interactions in setup (such as
evaluating 1000 stair configurations without having to save every one).

### Parameterized Tests

To make tests more general, many of them use parameterized inputs.
These inputs are controlled via the [NUnit Framework Attributes](https://docs.nunit.org/articles/nunit/writing-tests/attributes.html)
such as [Range](https://docs.nunit.org/articles/nunit/writing-tests/attributes/range.html)
or [Values](https://docs.nunit.org/articles/nunit/writing-tests/attributes/values.html).

See some tests such as [KCCUtilsScenarioTests](xref:nickmaltbie.OpenKCC.Tests.PlayMode.KCCUtilsScenarioTests)
for examples.

## Coverage Reports

Unity's [Code Coverage](https://docs.unity3d.com/Packages/com.unity.testtools.codecoverage@1.2/manual/index.html)
package provides a way to visualize which lines of code are covered
by unit tests and evaluate the quality of the testing in the project.
The coverage results can be generated whenever running tests
as long as coverage tracking is enabled. Please see docs on the
[Code Coverage Window](https://docs.unity3d.com/Packages/com.unity.testtools.codecoverage@1.2/manual/CodeCoverageWindow.html)
for further details.

These coverage results are also generated as part of each pull request and build
of the project.

### Coverage Requirements

Some projects may require a specific percentage of code coverage such as 95\%,
but I find this to be a bit silly as requiring a
percentage of lines covered doesn't really make sense.

EditMode tests should be able to reach every line of code
as if a line of code is not reachable in a test,
why is it in the project.

PlayMode tests should encompass every major feature
and scenario to ensure they are not broken with future
updates.

Adding tests may need to be a separate step from adding
code as some testing frameworks require much more
time and effort to add to the project.
