---
layout: page
subheadline: "LLVM"
title: "Codechecker - Getting Started"
teaser: "CodeChecker is an open-source static code analysis tool developed as part of the LLVM project. Here's a comprehensive introduction to the basics of CodeChecker. The contents of this post have been derived from various presentations and tutorials created by LLVM developers and conferences"
header:
    image_fullwidth: "pages/technical/llvm/llvm-header.png"
image:
    thumb:  posts/technical/llvm/llvm_logo.png
    homepage: posts/technical/llvm/llvm_logo.png
categories:
    - llvm
    - technical
tags:
    - codechecker
comments: true
show_meta: false
breadcrumb: true
---

## About CodeChecker
CodeChecker is designed to help developers identify potential issues and bugs in their codebase by performing static analysis on source code. Here's a comprehensive introduction to the basics of CodeChecker:

1. Static Code Analysis: CodeChecker is primarily used for static code analysis, which involves examining source code without executing it. This analysis helps identify issues such as coding errors, potential security vulnerabilities, coding standard violations, and performance inefficiencies.
2. Integration with Clang Static Analyzer: CodeChecker integrates with Clang Static Analyzer, a part of the LLVM project. Clang Static Analyzer performs a deep analysis of C, C++, and Objective-C code to identify bugs and potential problems.
3. Supported Languages: CodeChecker supports multiple programming languages, including C, C++, and Objective-C. This makes it suitable for projects written in these languages, such as system software, applications, and libraries.
4. Supported Platforms: CodeChecker is cross-platform and can be used on various operating systems, including Linux, macOS, and Windows. This ensures flexibility and accessibility for developers working on different platforms.
5. Workflow Integration: CodeChecker seamlessly integrates into the development workflow, allowing developers to incorporate static code analysis into their existing processes. It supports various build systems, version control systems, and continuous integration (CI) tools, enabling easy integration into the development pipeline.
6. User-Friendly Interface: CodeChecker provides a user-friendly web interface that allows developers to view analysis results, browse detected issues, and prioritize fixes. The interface includes features such as filtering, sorting, and grouping to facilitate efficient issue management.
7. Issue Tracking and Reporting: CodeChecker generates detailed reports of detected issues, including descriptions, locations in the code, severity levels, and suggested fixes. Developers can track the status of issues, assign them to team members, and monitor progress towards resolution.
8. Customization and Configuration: CodeChecker offers various customization options to tailor the analysis process to specific project requirements. Developers can configure analysis parameters, define custom rulesets, and adjust severity thresholds to meet their needs.
9. Scalability and Performance: CodeChecker is designed to handle large codebases efficiently, ensuring scalability and performance even in complex projects. It employs advanced analysis techniques and optimizations to minimize analysis time and resource usage.
10. Community and Support: CodeChecker is an open-source project with an active community of developers and contributors. Users can access documentation, forums, and mailing lists for support, as well as contribute to the project by reporting issues, submitting patches, or sharing feedback.


## Getting Started
CodeChecker needs compile_commands.json file which contains the commands along with CLI parameters neeeded for compilation. 
Codechecker supports the following operations:
### Log:
- Used to intercept compiler calls and generate compile_commands.json file. For eg. bitbake can be used to intercept compiler calls

### Analyze:
- Run the configured analyzers on the source code. This step outputs the plist file

### Parse:
- Parse the analysis results and print an overview of the analysis results to the terminal

### Store:
- Upload the analysis results to the codechecker server to view in a user friendly format

#### All the steps above take a configuration file with set of options as input


## Codechecker CLI Options
- analyze
    -  Execute the supported code analyzers for the files recorded in a JSON Compilation Database
- analyzer-version
    - Print the version of CodeChecker analyzer package that is being used.
- analyzers
    - List supported and available analyzers
- check
    - Perform analysis on a project and print results to standard output
- checkers
    - List the checkers available for code analysis
- cmd
    - View analysis results on a running server from the command line
    - SUBCOMMANDS:
        - runs 
            - List the available analysis runs
        - history
            - Show run history of multiple runs
        - results
            - List analysis result (finding) summary for a given run
        - diff
            - Compare two analysis runs and show the difference
        - sum
            - Show statistics of checkers
        - token
            - Access subcommands related to configuring personal access tokens managed by a CodeChecker server
        - del
            - Delete analysis runs
        - update
            - Update an analysis run
        - suppress
            - Manage and import suppressions of reports on a CodeChecker server
        - products
            - Access subcommands related to configuring the products managed by a CodeChecker server
        - components
            - Access subcommands related to configuring the source components managed by a CodeChecker server
        - login
            - Authenticate into CodeChecker servers that require privileges
        - export
            - Export comments and review statuses from CodeChecker
        - import
            - Import comments and review statuses into CodeChecker
- fixit
    - Apply automatic fixes based on the suggestions of the analyzers
- log
    - Run a build command, collect the executed compilation commands and store them in a JSON file.
- parse
    - Print analysis summary and results in a human-readable format
- server
    - Start and manage the CodeChecker Web server.
- store
    - Save analysis results to a database.
- version
    - Print the version of CodeChecker package that is being used
- web-version
    - Print the version of CodeChecker server package that is being used


## Static Analyzer Configuration
- Command to pass the configuration file to CodeChecker
    - CodeChecker analyze --saargs static_analyzer.cfg
- saargs forwards arguments without modification to the Clang Static Analyzer
- Before every configuration option '-Xclang' argument should be written 
- All the configuration options should be in one line
- Static analyzer and checker related configuration options can be configured
- Sample line entry in the config file
    - -Xclang -analyzer-config -Xclang unix.Malloc:Optimistic=true -Xclang -analyzer-max-loop -Xclang 20


## Clang tidy configuration
- clang-tidy attempts to read configuration for each analyzed source file from a .clang-tidy file located in the closest parent directory of the analyzed source file
- .clang-tidy configuration file can be in JSON or YAML format
- Used to configure
    - Checkers run along with their custom setting values
- Command to pass the configuration file to CodeChecker
    - CodeChecker analyze --tidyargs tidy_analyzer.cfg
- Sample config file contents
    - ![Debug Report]({{site.urlimg}}posts\technical\llvm\codechecker-getting-started\clang-tidy-config.png)
	
		
