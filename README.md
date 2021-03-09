# Pitest github integration

This is an early demo of [pitest](https://pitest.org/)/github pull request integration.

If you'd be interested in early access please contact `pitest.demo@groupcdg.com`

Take a look at the [open PRs](https://github.com/GroupCDG-Labs/pitest-github-demo/pulls) to see it in action.

Note that while the plugins work well for PRs in branches of the same repo (as used by most teams for private repos) the GitHub permissions model means the plugin does not have sufficient access when run from PRs from forked repos.

## How it works

### Low level git integration

The `pitest-git-plugin` extension to pitest adds functionality to map between bytecode and source files within git version control. The allows the scope of mutation analysis to be easily limited to just a portion of changed code, such as a pull request.

The plugin can also be used locally. If you clone this repo and run :-

`mvn -Ppitest -Dfeatures=+GIT test`

Then analyses will be limited to lines of code that have been modified locally. This gives an ideal way to get feedback on your work before pushing.

The plugin is commercial software and requires a licence from CDG to be used. This project uses an embedded demo licence which may only be used with code in the package `com.example`. You are free to use this demo licence for the purpose of evaluation and demonstration, but not for commercial development.

Free licences are available for open source projects.

### Github integration

The project is configured to run the new maven goal `pitest-github:github` for each pull request, via github actions.

This goal reads files generated by the `pitest-git-plugin` and uses them to create a github check run. The results from multiple modules are consolidated to give a single view.

A comment is added to the PR each time the goal is run containing a high level overview of the killed and surviving mutants. Annotations are added to each line of code showing surviving mutants.

This PR workflow allows mutation testing to be used effectively on projects where it would not be practical to mutate the entire codebase. Gaps in the test suite and redundant code will be highlighted automatically each time an area of code is changed.

Currently, it is possible to accidentally reduce the effectiveness of a test suite by removing tests without modifying the covered code, which would not be detected by the PR workflow. This may be addressed in the future.


