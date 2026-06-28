# General Test Techniques

A test technique is a heuristic for designing tests. The nine families below are general enough to apply across a wide variety of contexts. An endless variety of specific test techniques may be constructed by combining one or more of these families with coverage ideas from the Product Elements and Quality Criteria lists.

Source: **Heuristic Test Strategy Model (HTSM)** by James Bach, v6.0.

---

## Function Testing — *Test what it can do*

1. Identify things that the product can do (functions and subfunctions).
2. Determine how you'd know if a function was capable of working.
3. Test each function, one at a time.
4. See that each function does what it's supposed to do and not what it isn't supposed to do.

---

## Claims Testing — *Challenge every claim*

1. Identify reference materials that include claims about the product (tacit or explicit). Consider SLAs, EULAs, advertisements, specifications, help text, manuals, etc.
2. Analyse individual claims, and clarify vague claims.
3. Test each claim about the product.
4. If you're testing from an explicit specification, expect it and the product to be brought into alignment.

---

## Domain Testing — *Partition the data*

1. Look for any data processed by the product. Look at outputs as well as inputs.
2. Decide which particular data to test with. Consider things like boundary values, typical values, convenient values, invalid values, or best representatives.
3. Consider combinations of data worth testing together.
4. Consider using inputs that force the whole range of possible outputs to occur.

---

## User Testing — *Involve the users*

1. Identify categories and roles of users.
2. Determine what each category of user will do (use cases), how they will do it, and what they value.
3. Get real user data, logs based on user activity, or bring real users in to test.
4. Otherwise, systematically simulate a user (be careful — it's easy to think you're like a user even when you're not).
5. Powerful user testing involves a variety of users and user roles, not just one.

---

## Stress Testing — *Overwhelm the product*

1. Look for sub-systems and functions that are vulnerable to being overloaded or "broken" in the presence of challenging data or constrained resources.
2. Identify data and resources related to those sub-systems and functions.
3. Select or generate challenging data, or resource constraint conditions to test with: e.g. large or complex data structures, high loads, long test runs, many test cases, low memory conditions.

---

## Risk Testing — *Imagine a problem, then look for it*

1. What kinds of problems could the product have?
2. Which kinds matter most? Focus on those.
3. How would you detect them if they were there?
4. Make a list of interesting problems and design tests specifically to reveal them.
5. It may help to consult experts, design documentation, past bug reports, or apply risk heuristics.

---

## Flow Testing — *Do one thing after another*

1. Perform multiple activities connected end-to-end; for instance, conduct tours through a state model.
2. Don't reset the system between actions.
3. Vary timing and sequencing, and try parallel threads.

---

## Scenario Testing — *Test to a compelling story*

1. Begin by thinking about everything going on around the product.
2. Design tests that involve meaningful and complex interactions with the product.
3. A good scenario test is a compelling story of how someone who matters might do something that matters with the product.

---

## Tool-Supported Testing — *Use tools to make testers more powerful*

1. Look for or develop tools that can perform a lot of actions and check a lot of things.
2. Consider tools that partially automate test coverage.
3. Consider tools that partially automate oracles.
4. Consider automatic change detectors.
5. Consider automatic test data generators.
