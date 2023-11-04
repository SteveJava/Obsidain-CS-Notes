>Concurrent Programming correctly and efficiently controlling access by multiple [[threads]] to shared resources

- Different tasks may **need to happen at once** for program to work
- Simultaneous tasks may be necessary for: 
	- **responsiveness**
	- **efficient processor utilisation**
	- Failure isolation
- [[Synchronisation]] is needed to order events in concurrent programs

## Concurrency Problems

- **Race Conditions**: an *error* in a program where the output and/or result of the process is *unexpectedly* and *critically* dependent on the relative sequence or timing of the events

- The idea: events *race* each other to influence the output first

