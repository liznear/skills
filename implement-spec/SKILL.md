---
name: implement-spec
description: How to safely implement a software feature from a specification document. Make sure to use this skill whenever a user gives you a spec, asks you to implement a new feature from a document, or asks for test-driven development based on requirements.
---

# Implement Spec Workflow

You have been asked to implement a new feature or set of code based on a specification document. To ensure high-quality, correct, and testable code, you MUST follow this exact sequence of steps.

## Step 1: Read the Spec & Clarify Ambiguities
- Read the specification document provided by the user very carefully.
- Look for edge cases, undefined behavior, or missing data structures.
- **CRITICAL**: If there is *any* ambiguity or unclear requirement, DO NOT guess. Stop and ask the user for clarification.
- Once the user answers, **update the specification document** with the new information so it serves as the ground truth.

## Step 2: Define Data Types and Interfaces
- Create or modify files to define the data structures, types, and interfaces required by the spec.
- **DO NOT** write any functional implementation logic yet.
- This step ensures the domain model is solid and aligns with the spec before getting into the weeds of implementation.

## Step 3: Write Tests
- Write comprehensive tests covering happy paths, edge cases, and the error conditions defined in the spec.
- The tests should rely on the types and interfaces you just defined.
- These tests will naturally fail right now because there is no implementation. That is expected.

## Step 4: Incremental Implementation
- Begin implementing the code required to make the tests pass.
- Do this **incrementally**. Implement one logical chunk or one failing test at a time, then run the tests to verify your progress.
- Continue iterating on the implementation and running tests until all tests pass and the specification is fully satisfied.
