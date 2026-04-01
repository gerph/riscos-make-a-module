There is a RISC OS module calls EasySocket in the file EasySocket,ffa.

I would like you to...

1. Work out what it does and how it works.
2. Describe what you have found in a file `FINDINGS.md`. Commit the changes.
3. Create a new RISC OS C module skeleton. Build the module. Commit the changes.
4. Implement each SWI call from your findings in turn. After each implementation, commit the changes.
5. Implement any callbacks and handlers that you need. After each, commit the changes.
6. Implement the *Commands that the module provides. After each, commit the changes.

If you run any tests you will need a timeout to ensure that the test doesn't hang forever. Most operations will take < 10 seconds. Never run any test for longer 
than 60 seconds. The `riscos-build-run` command takes a timeout parameter with `-t <secs>`.

Each change should be documented in the commit properly, and keep a running log of the features that are added in `WORK.md`.
Build the module after each feature is added.
After every commit push the change.

If there's something you don't know, but can stub, do so. If you need input from the user, ask a question.
