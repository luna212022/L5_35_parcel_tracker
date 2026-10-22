# Reflection

(The handout PDF I was given did not list the reflection questions, so these are the usual four for this lab. Match them to your handout's wording.)

**1. Which issues were easiest and hardest to fix?**
Easiest: Flake8 formatting and Pylint naming/docstrings, because they are mechanical. Hardest: the Bandit shell-injection issue, because the fix needed a design change (validate the input and pass arguments as a list), not just a rename.

**2. Did the tools report anything that was a false positive or not worth fixing?**
Bandit B404 (importing subprocess) is informational. Once the call was made safe I kept the import and suppressed it with `# nosec` plus a reason instead of removing a legitimately needed module. Pylint's docstring warnings on tiny helpers feel pedantic, but they are cheap to satisfy.

**3. How would you use static analysis in a real workflow?**
Run Flake8 and Pylint as a pre-commit hook, and run all three in CI on every pull request so bad code fails the build before review. Bandit especially should be a merge gate because it catches security bugs that tests rarely cover.

**4. What improved in the code after the fixes?**
Pylint went 4.91 to 10.00, Bandit went from 4 findings (1 High) to 0, and Flake8 from 11 to 0. The mutable default argument bug (shared `history` list) is gone, the ping helper can no longer be used for command injection, the temp file is created safely, the OTP uses a cryptographic generator, and the code reads consistently. Program output is identical to before.
