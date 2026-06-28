# Project Environment

Creating and executing tests is the heart of the test project. However, many factors in the project environment are critical to decisions about what specific tests to create. Consider how each element may help or hinder your test design process, and try to exploit every resource.

Source: **Heuristic Test Strategy Model (HTSM)** by James Bach, v6.0.

---

## Mission — *Your purpose on this project, as understood by you and your customers*

- Why are you testing? Are you motivated by a general concern about quality or specific and defined risks?
- Do you know who the customers of your work are? Whose opinions matter? Who benefits or suffers from the work you do?
- Maybe the people you serve have strong ideas about what tests you should create and run. Find out.
- Have you negotiated project conditions that affect your ability to accept your mission?

---

## Information — *Information about the product or project that is needed for testing*

- Whom can we consult with to learn about this project?
- Are there any engineering documents available? User manuals? Web-based materials? Specs? User stories?
- Does this product have a history? Old problems that were fixed or deferred? Pattern of customer complaints?
- Is your information current? How are you apprised of new or changing information?
- Are there any comparable products or projects from which we can glean important information?

---

## Developer Relations — *How you get along with the programmers*

- **Rapport**: have you developed a friendly working relationship with the programmers?
- **Hubris**: does the development team seem overconfident about any aspect of the product?
- **Defensiveness**: is there any part of the product the developers seem strangely opposed to having tested?
- **Feedback Loop**: can you communicate quickly, on demand, with the programmers?
- **Feedback**: what do the developers think of your test strategy?

---

## Test Team — *Anyone who will perform or support testing*

- Do you know who will be testing? Do they have the knowledge and skills they need?
- Are there people not on the "test team" that might be able to help? People who've tested similar products before and might have advice? Writers? Users? Programmers?
- Are there particular test techniques that someone on the team has special skill or motivation to perform?
- Who is co-located and who is elsewhere? Will time zones be a problem?

---

## Equipment & Tools — *Hardware, software, or documents required to administer testing*

- **Hardware**: do you have all the physical or virtual hardware you need for testing? Do you control it or share it?
- **Automated Checking**: do you have tools that allow you to control and observe product behaviour automatically?
- **Analytical Tools**: do you have tools to create test data, design scenarios, or to analyse and track test results?
- **Matrices & Checklists**: are any documents needed to track or record the progress of testing?
- **Signals**: do you have access to engineering data coming back from the field?

---

## Schedule — *The sequence, duration, and synchronisation of project events*

- **Test Design**: how much time do you have? Are there tests better to create later than sooner?
- **Test Execution**: when will tests be performed? Are some tests performed repeatedly, say, for regression purposes?
- **Development**: when will builds be available for testing, features added, code frozen, etc.?
- **Documentation**: when will the user documentation be available for review?

---

## Test Items — *The product to be tested*

- **Scope**: what parts of the product are and are not within the scope of your testing responsibility?
- **Availability**: do you have the product to test? Do you have test platforms available? Will you test in production?
- **Interoperable Systems**: are any third-party services required for your product that must be mocked or made available?
- **Volatility**: is the product constantly changing? How will you find out about changes?
- **New Stuff**: do you know what has recently been changed or added in the product?
- **Testability**: is the product functional and reliable enough that you can effectively test it?
- **Future Releases**: what part of your testing, if any, must be designed to apply to future releases of the product?

---

## Deliverables — *The observable products of the test project*

- **Content**: what sort of reports will you have to make? Will you share your working notes, or just the end results?
- **Purpose**: are your deliverables provided as part of the product? Does anyone else have to run your tests?
- **Standards**: is there a particular test documentation standard you're supposed to follow?
- **Media**: how will you record and communicate your reports?
