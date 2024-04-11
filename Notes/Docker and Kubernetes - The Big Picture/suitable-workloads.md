# Suitable Workload

This file contains notes for the "**Suitable Workloads**" section of the [Docker and Kubernetes: The Big Picture](https://app.pluralsight.com/library/courses/docker-kubernetes-big-picture/table-of-contents) PluralSight Class by Nigel Poulton.

## Summary

- Technology is always about either the business or the project or both.
- Despite the effort required, for the sake of yourself & your business, you should start using containers.
- Containers are great at Stateless and Stateful.
- Instead of just migrating old apps into containers, you should either get rid of that old app and get a new better app, or refactor that old app into something better.

## ----------------------- Deeper Explanation -----------------------

## Suitable Workloads-

- Containers started off being great at Stateless but it was only okay at Stateful.
- This has changed so now Containers are great at both.
- Stateful means it has to remember things. Like a database, if it crashes or restarts then it has to remember the data it had stored & where the application was before it crashed/restarted.
- Stateless doesn't remember anything after it crashes/restarts. This is great for static content because it doesn't need to change so it can redo everything on each restart.

## Low-hanging Fruit

- If you migrate a Legacy app into a container, that's great because it doesn't waste as much resources anymore, but you still have an old and slow Legacy app and it doesn't help your business that much.
- Instead of just migrating a Legacy app into a container, you should find a way to either not use the app & go with something else that is better, or to refactor the Legacy app if you need to keep it.
- Getting new apps and/or refactoring old apps will allow your business to adapt and thrive.
- Getting new apps and/or refactoring old apps is a two edged sword because yes it is great and helps you thrive, but it takes a decent amount of work to set up.
