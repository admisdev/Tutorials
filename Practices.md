# ADMIS Software Development Practices

## Purpose and Scope
This document means to provide a guideline on software development practices. All IT Development work at ADM Investor Services need to follow this guideline. This is a living document and will be updated when we progress.

## Document
1.	Documentation need to be done before the coding start. It should be ready for design review.
2.	The document should include the following:
a.	High level function specification
b.	Architecture

## Code Branching and Merging
All development work need have the code branched according to project, task or feature request, with the exception of absolutely emergency fixes.
1.	Master branch should always have the latest production code.
2.	All large development tasks and projects need to be done in its dedicated branch.
3.	A BAU_DEV should serve as maintenance branch for regular accumulative release of small improvement and bug fixes
4.	If multiple branches need to be released at the time, merge all to BAU_DEV first, build and testing.
5.	Merge code early and often depends on release and development schedule

## TDD (Test Driven Development)
1.	Regardless, all development work need to be adequately tested on Unit and System level.
2.	At this moment, unit tests are not required for all functions but we need to start use it and gradually improve coverage on our code.
3.	Please be aware that TDD actually could be a good aid on design.

## Code and Design Review
We are taking a two tier approach on code review to balance efficiency and thoroughness.
1.	Regular Code and Design Review
a.	On a weekly base when you are working on an active project
b.	Lead developer need to organize this review
c.	Areas to cover: security, architecture, design and code
d.	A summary need to be sent to the whole Development Team
2.	Full code review
a.	Make sure we are following the coding standard
b.	Make sure we have more developers involved with the right expertise in architecture, design and information security
c.	When you have 200-300 lines code to walk through
d.	When a high risk and critical components are touched
i.	Authentication and authorization
ii.	Exchange APIs for trading platform
iii.	Pre-Trade risk components for trading platform
iv.	Cash manager
v.	Any code affect distribution of customer data to large number of users
e.	A summary need to be sent to the whole Development Team

## Follow Release Management Process
Will be defined in the doc at: http://admiscentral/departments/IT/Development/Development%20Process%20and%20Tools/SoftwareDevelopmentProcedures.docx


## References:

### Coding Standards

C#: In principal, we follow the coding standard at https://msdn.microsoft.com/en-us/library/ff926074.aspx with exception of use of “Implicity Typed Local Variables” (var) type. 

JavaScript: https://www.drupal.org/node/172169    https://make.wordpress.org/core/handbook/best-practices/coding-standards/javascript/

C++: http://www.umich.edu/~eecs381/handouts/C++_Coding_Standards.pdf

No SQL Standard as we want to use Database as it intended to be, as a storage engine first.
