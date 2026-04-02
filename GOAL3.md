1. Check the disassembly again and see whether each of the entry points do the right things in the new C code (no code changes yet):
    * Module initialisation.
    * Module finalisation.
    * SWI handler.
    * Service handler.
2. Check any SWI calls that are used within the module and ensure that they are also used correctly in the new C code. (no code changes yet)
3. Update the FINDINGS.md with the additional information that you have discovered. Note down any parts of the disassembly that you do not understand in the file.
4. Update WORK.md with the list of deficiencies you have found.
5. Address the issues.

After each step or major change, commit the code with a descriptive message and push.

