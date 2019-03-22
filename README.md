# Aim of DocuMesh
DocuMesh aims to automate documentation as part of the code as far as possible to reduce the cost of manual documentation.

# Concepts
## Generic Entrypoint abstraction
### Introduction
Technical documentations usually suffer from accessibility issues. 
Let's imagine you recently have been added to a new team or you have just switched your job and need to get familiar with the existing software. In this cases it can be quite challenging to adept to the software as you usually lack knowledge about:

- business processes
- used architecture
- naming conventions

The result being that in case of a bug or change request for an existing feature you will need a lot of time for analysis of the code and you need to make assumptions about the intentions behind the code.

### Precondition
A strong focus on behaviour driven tests is necessary.

### Solution
Let's assume the project is mainly tested with behaviour driven tests and these tests relate to project requirements.
First we need to execute every test and accumulate every traversed line of code. The traversed lines of code and the corresponding classes need to be saved in relation to the executed test afterwards.

The generated **requirement-test-class-code model** can than be used to access the code from different entrypoints.
I.e:
- get all requirements involved for a certain line of code or class to estimate the consequences of a code change
- get all classes necessary for a certain requirement to get an overview of involved classes
- get all requirements or tests for a certain line of code  in case of a bug to get information about the intention behind the code, thus fixing a bug in a more informed way.

## Mocking with BDD
### Introduction
BDD Frameworks like cucumber are usually designed to work well with frontends but not with mocked backend tests. For this it's necessary to share the State of the test between the test steps. Unfortunately this is not possible out of the box and you'd need to resort to global variables in step libraries, polluting the actual test code.

### Solution
A small scale object to step storage that can be reset for each complete test but keeps the data between single steps of the same test.
For ease of use it should feature a fluent api like the following example:

- initialize
`ScenarioResult.initialize();`
- add single result
`ScenarioResult.addResult(object).toGivenStep();`
- add result list
`ScenarioResult.addResultListofType(Object.class).with(objectList).toGivenStep();`
- get single result
`ScenarioResult.getResultofType(Object.class).fromGivenStep().asSingleResult();`
- get result list
`ScenarioResult.getResultofType(Object.class).fromGivenStep().asResultList();`
- get single result from resultList
`ScenarioResult.getResultofType(Object.class).fromGivenStep().asSingleResult(new EntityIdentifier<Object>() {...});`
