> 🚨 Note: This log may contain personal information such as the contents of your files or terminal output. Please review the contents carefully before sharing.
# panel/editAgent - e0d6d98a

- [Request Messages](#request-messages)
  - [System](#system)
  - [User](#user)
- [Response](#response)

## Metadata
~~~
requestType      : ChatCompletions
model            : claude-sonnet-4.5
maxPromptTokens  : 127997
maxResponseTokens: 16000
location         : 7
otherOptions     : {"temperature":0,"stream":true}
intent           : undefined
startTime        : 2026-01-29T21:04:29.975Z
endTime          : 2026-01-29T21:04:35.965Z
duration         : 5990ms
ourRequestId     : 989c6414-4be2-4c72-81d2-c3e29bb0495f
requestId        : 989c6414-4be2-4c72-81d2-c3e29bb0495f
serverRequestId  : 989c6414-4be2-4c72-81d2-c3e29bb0495f
timeToFirstToken : 1073ms
resolved model   : claude-sonnet-4.5
usage            : {"completion_tokens":410,"prompt_tokens":23259,"prompt_tokens_details":{"cached_tokens":0},"total_tokens":23669}
tools            : [
    {
        "function": {
            "name": "create_directory",
            "description": "Create a new directory structure in the workspace. Will recursively create all directories in the path, like mkdir -p. You do not need to use this tool before using create_file, that tool will automatically create the needed directories.",
            "parameters": {
                "type": "object",
                "properties": {
                    "dirPath": {
                        "type": "string",
                        "description": "The absolute path to the directory to create."
                    }
                },
                "required": [
                    "dirPath"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "create_file",
            "description": "This is a tool for creating a new file in the workspace. The file will be created with the specified content. The directory will be created if it does not already exist. Never use this tool to edit a file that already exists.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "The absolute path to the file to create."
                    },
                    "content": {
                        "type": "string",
                        "description": "The content to write to the file."
                    }
                },
                "required": [
                    "filePath",
                    "content"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "create_new_jupyter_notebook",
            "description": "Generates a new Jupyter Notebook (.ipynb) in VS Code. Jupyter Notebooks are interactive documents commonly used for data exploration, analysis, visualization, and combining code with narrative text. Prefer creating plain Python files or similar unless a user explicitly requests creating a new Jupyter Notebook or already has a Jupyter Notebook opened or exists in the workspace.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The query to use to generate the jupyter notebook. This should be a clear and concise description of the notebook the user wants to create."
                    }
                },
                "required": [
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "create_new_workspace",
            "description": "Get comprehensive setup steps to help the user create complete project structures in a VS Code workspace. This tool is designed for full project initialization and scaffolding, not for creating individual files.\n\nWhen to use this tool:\n- User wants to create a new complete project from scratch\n- Setting up entire project frameworks (TypeScript projects, React apps, Node.js servers, etc.)\n- Initializing Model Context Protocol (MCP) servers with full structure\n- Creating VS Code extensions with proper scaffolding\n- Setting up Next.js, Vite, or other framework-based projects\n- User asks for \"new project\", \"create a workspace\", \"set up a [framework] project\"\n- Need to establish complete development environment with dependencies, config files, and folder structure\n\nWhen NOT to use this tool:\n- Creating single files or small code snippets\n- Adding individual files to existing projects\n- Making modifications to existing codebases\n- User asks to \"create a file\" or \"add a component\"\n- Simple code examples or demonstrations\n- Debugging or fixing existing code\n\nThis tool provides complete project setup including:\n- Folder structure creation\n- Package.json and dependency management\n- Configuration files (tsconfig, eslint, etc.)\n- Initial boilerplate code\n- Development environment setup\n- Build and run instructions\n\nUse other file creation tools for individual files within existing projects.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The query to use to generate the new workspace. This should be a clear and concise description of the workspace the user wants to create."
                    }
                },
                "required": [
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "edit_notebook_file",
            "description": "This is a tool for editing an existing Notebook file in the workspace. Generate the \"explanation\" property first.\nThe system is very smart and can understand how to apply your edits to the notebooks.\nWhen updating the content of an existing cell, ensure newCode preserves whitespace and indentation exactly and does NOT include any code markers such as (...existing code...).",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "An absolute path to the notebook file to edit, or the URI of a untitled, not yet named, file, such as `untitled:Untitled-1."
                    },
                    "cellId": {
                        "type": "string",
                        "description": "Id of the cell that needs to be deleted or edited. Use the value `TOP`, `BOTTOM` when inserting a cell at the top or bottom of the notebook, else provide the id of the cell after which a new cell is to be inserted. Remember, if a cellId is provided and editType=insert, then a cell will be inserted after the cell with the provided cellId."
                    },
                    "newCode": {
                        "anyOf": [
                            {
                                "type": "string",
                                "description": "The code for the new or existing cell to be edited. Code should not be wrapped within <VSCode.Cell> tags. Do NOT include code markers such as (...existing code...) to indicate existing code."
                            },
                            {
                                "type": "array",
                                "items": {
                                    "type": "string",
                                    "description": "The code for the new or existing cell to be edited. Code should not be wrapped within <VSCode.Cell> tags"
                                }
                            }
                        ]
                    },
                    "language": {
                        "type": "string",
                        "description": "The language of the cell. `markdown`, `python`, `javascript`, `julia`, etc."
                    },
                    "editType": {
                        "type": "string",
                        "enum": [
                            "insert",
                            "delete",
                            "edit"
                        ],
                        "description": "The operation peformed on the cell, whether `insert`, `delete` or `edit`.\nUse the `editType` field to specify the operation: `insert` to add a new cell, `edit` to modify an existing cell's content, and `delete` to remove a cell."
                    }
                },
                "required": [
                    "filePath",
                    "editType",
                    "cellId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "fetch_webpage",
            "description": "Fetches the main content from a web page. This tool is useful for summarizing or analyzing the content of a webpage. You should use this tool when you think the user is looking for information from a specific webpage.",
            "parameters": {
                "type": "object",
                "properties": {
                    "urls": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "description": "An array of URLs to fetch content from."
                    },
                    "query": {
                        "type": "string",
                        "description": "The query to search for in the web page's content. This should be a clear and concise description of the content you want to find."
                    }
                },
                "required": [
                    "urls",
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "file_search",
            "description": "Search for files in the workspace by glob pattern. This only returns the paths of matching files. Use this tool when you know the exact filename pattern of the files you're searching for. Glob patterns match from the root of the workspace folder. Examples:\n- **/*.{js,ts} to match all js/ts files in the workspace.\n- src/** to match all files under the top-level src folder.\n- **/foo/**/*.js to match all js files under any foo folder in the workspace.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "Search for files with names or paths matching this glob pattern."
                    },
                    "maxResults": {
                        "type": "number",
                        "description": "The maximum number of results to return. Do not use this unless necessary, it can slow things down. By default, only some matches are returned. If you use this and don't see what you're looking for, you can try again with a more specific query or a larger maxResults."
                    }
                },
                "required": [
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "grep_search",
            "description": "Do a fast text search in the workspace. Use this tool when you want to search with an exact string or regex. If you are not sure what words will appear in the workspace, prefer using regex patterns with alternation (|) or character classes to search for multiple potential words at once instead of making separate searches. For example, use 'function|method|procedure' to look for all of those words at once. Use includePattern to search within files matching a specific pattern, or in a specific file, using a relative path. Use 'includeIgnoredFiles' to include files normally ignored by .gitignore, other ignore files, and `files.exclude` and `search.exclude` settings. Warning: using this may cause the search to be slower, only set it when you want to search in ignored folders like node_modules or build outputs. Use this tool when you want to see an overview of a particular file, instead of using read_file many times to look for code within a file.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The pattern to search for in files in the workspace. Use regex with alternation (e.g., 'word1|word2|word3') or character classes to find multiple potential words in a single search. Be sure to set the isRegexp property properly to declare whether it's a regex or plain text pattern. Is case-insensitive."
                    },
                    "isRegexp": {
                        "type": "boolean",
                        "description": "Whether the pattern is a regex."
                    },
                    "includePattern": {
                        "type": "string",
                        "description": "Search files matching this glob pattern. Will be applied to the relative path of files within the workspace. To search recursively inside a folder, use a proper glob pattern like \"src/folder/**\". Do not use | in includePattern."
                    },
                    "maxResults": {
                        "type": "number",
                        "description": "The maximum number of results to return. Do not use this unless necessary, it can slow things down. By default, only some matches are returned. If you use this and don't see what you're looking for, you can try again with a more specific query or a larger maxResults."
                    },
                    "includeIgnoredFiles": {
                        "type": "boolean",
                        "description": "Whether to include files that would normally be ignored according to .gitignore, other ignore files and `files.exclude` and `search.exclude` settings. Warning: using this may cause the search to be slower. Only set it when you want to search in ignored folders like node_modules or build outputs."
                    }
                },
                "required": [
                    "query",
                    "isRegexp"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_changed_files",
            "description": "Get git diffs of current file changes in a git repository. Don't forget that you can use run_in_terminal to run git commands in a terminal as well.",
            "parameters": {
                "type": "object",
                "properties": {
                    "repositoryPath": {
                        "type": "string",
                        "description": "The absolute path to the git repository to look for changes in. If not provided, the active git repository will be used."
                    },
                    "sourceControlState": {
                        "type": "array",
                        "items": {
                            "type": "string",
                            "enum": [
                                "staged",
                                "unstaged",
                                "merge-conflicts"
                            ]
                        },
                        "description": "The kinds of git state to filter by. Allowed values are: 'staged', 'unstaged', and 'merge-conflicts'. If not provided, all states will be included."
                    }
                }
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_errors",
            "description": "Get any compile or lint errors in a specific file or across all files. If the user mentions errors or problems in a file, they may be referring to these. Use the tool to see the same errors that the user is seeing. If the user asks you to analyze all errors, or does not specify a file, use this tool to gather errors for all files. Also use this tool after editing a file to validate the change.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePaths": {
                        "description": "The absolute paths to the files or folders to check for errors. Omit 'filePaths' when retrieving all errors.",
                        "type": "array",
                        "items": {
                            "type": "string"
                        }
                    }
                }
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "copilot_getNotebookSummary",
            "description": "This is a tool returns the list of the Notebook cells along with the id, cell types, line ranges, language, execution information and output mime types for each cell. This is useful to get Cell Ids when executing a notebook or determine what cells have been executed and what order, or what cells have outputs. If required to read contents of a cell use this to determine the line range of a cells, and then use read_file tool to read a specific line range. Requery this tool if the contents of the notebook change.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "An absolute path to the notebook file with the cell to run, or the URI of a untitled, not yet named, file, such as `untitled:Untitled-1.ipynb"
                    }
                },
                "required": [
                    "filePath"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_project_setup_info",
            "description": "Do not call this tool without first calling the tool to create a workspace. This tool provides a project setup information for a Visual Studio Code workspace based on a project type and programming language.",
            "parameters": {
                "type": "object",
                "properties": {
                    "projectType": {
                        "type": "string",
                        "description": "The type of project to create. Supported values are: 'python-script', 'python-project', 'mcp-server', 'model-context-protocol-server', 'vscode-extension', 'next-js', 'vite' and 'other'"
                    }
                },
                "required": [
                    "projectType"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_search_view_results",
            "description": "The results from the search view"
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_vscode_api",
            "description": "Get comprehensive VS Code API documentation and references for extension development. This tool provides authoritative documentation for VS Code's extensive API surface, including proposed APIs, contribution points, and best practices. Use this tool for understanding complex VS Code API interactions.\n\nWhen to use this tool:\n- User asks about specific VS Code APIs, interfaces, or extension capabilities\n- Need documentation for VS Code extension contribution points (commands, views, settings, etc.)\n- Questions about proposed APIs and their usage patterns\n- Understanding VS Code extension lifecycle, activation events, and packaging\n- Best practices for VS Code extension development architecture\n- API examples and code patterns for extension features\n- Troubleshooting extension-specific issues or API limitations\n\nWhen NOT to use this tool:\n- Creating simple standalone files or scripts unrelated to VS Code extensions\n- General programming questions not specific to VS Code extension development\n- Questions about using VS Code as an editor (user-facing features)\n- Non-extension related development tasks\n- File creation or editing that doesn't involve VS Code extension APIs\n\nCRITICAL usage guidelines:\n1. Always include specific API names, interfaces, or concepts in your query\n2. Mention the extension feature you're trying to implement\n3. Include context about proposed vs stable APIs when relevant\n4. Reference specific contribution points when asking about extension manifest\n5. Be specific about the VS Code version or API version when known\n\nScope: This tool is for EXTENSION DEVELOPMENT ONLY - building tools that extend VS Code itself, not for general file creation or non-extension programming tasks.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The query to search vscode documentation for. Should contain all relevant context."
                    }
                },
                "required": [
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "github_repo",
            "description": "Searches a GitHub repository for relevant source code snippets. Only use this tool if the user is very clearly asking for code snippets from a specific GitHub repository. Do not use this tool for Github repos that the user has open in their workspace.",
            "parameters": {
                "type": "object",
                "properties": {
                    "repo": {
                        "type": "string",
                        "description": "The name of the Github repository to search for code in. Should must be formatted as '<owner>/<repo>'."
                    },
                    "query": {
                        "type": "string",
                        "description": "The query to search for repo. Should contain all relevant context."
                    }
                },
                "required": [
                    "repo",
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "install_extension",
            "description": "Install an extension in VS Code. Use this tool to install an extension in Visual Studio Code as part of a new workspace creation process only.",
            "parameters": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string",
                        "description": "The ID of the extension to install. This should be in the format <publisher>.<extension>."
                    },
                    "name": {
                        "type": "string",
                        "description": "The name of the extension to install. This should be a clear and concise description of the extension."
                    }
                },
                "required": [
                    "id",
                    "name"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "list_code_usages",
            "description": "Request to list all usages (references, definitions, implementations etc) of a function, class, method, variable etc. Use this tool when \n1. Looking for a sample implementation of an interface or class\n2. Checking how a function is used throughout the codebase.\n3. Including and updating all usages when changing a function, method, or constructor",
            "parameters": {
                "type": "object",
                "properties": {
                    "symbolName": {
                        "type": "string",
                        "description": "The name of the symbol, such as a function name, class name, method name, variable name, etc."
                    },
                    "filePaths": {
                        "type": "array",
                        "description": "One or more file paths which likely contain the definition of the symbol. For instance the file which declares a class or function. This is optional but will speed up the invocation of this tool and improve the quality of its output.",
                        "items": {
                            "type": "string"
                        }
                    }
                },
                "required": [
                    "symbolName"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "list_dir",
            "description": "List the contents of a directory. Result will have the name of the child. If the name ends in /, it's a folder, otherwise a file",
            "parameters": {
                "type": "object",
                "properties": {
                    "path": {
                        "type": "string",
                        "description": "The absolute path to the directory to list."
                    }
                },
                "required": [
                    "path"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "multi_replace_string_in_file",
            "description": "This tool allows you to apply multiple replace_string_in_file operations in a single call, which is more efficient than calling replace_string_in_file multiple times. It takes an array of replacement operations and applies them sequentially. Each replacement operation has the same parameters as replace_string_in_file: filePath, oldString, newString, and explanation. This tool is ideal when you need to make multiple edits across different files or multiple edits in the same file. The tool will provide a summary of successful and failed operations.",
            "parameters": {
                "type": "object",
                "properties": {
                    "explanation": {
                        "type": "string",
                        "description": "A brief explanation of what the multi-replace operation will accomplish."
                    },
                    "replacements": {
                        "type": "array",
                        "description": "An array of replacement operations to apply sequentially.",
                        "items": {
                            "type": "object",
                            "properties": {
                                "explanation": {
                                    "type": "string",
                                    "description": "A brief explanation of this specific replacement operation."
                                },
                                "filePath": {
                                    "type": "string",
                                    "description": "An absolute path to the file to edit."
                                },
                                "oldString": {
                                    "type": "string",
                                    "description": "The exact literal text to replace, preferably unescaped. Include at least 3 lines of context BEFORE and AFTER the target text, matching whitespace and indentation precisely. If this string is not the exact literal text or does not match exactly, this replacement will fail."
                                },
                                "newString": {
                                    "type": "string",
                                    "description": "The exact literal text to replace `oldString` with, preferably unescaped. Provide the EXACT text. Ensure the resulting code is correct and idiomatic."
                                }
                            },
                            "required": [
                                "explanation",
                                "filePath",
                                "oldString",
                                "newString"
                            ]
                        },
                        "minItems": 1
                    }
                },
                "required": [
                    "explanation",
                    "replacements"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "open_simple_browser",
            "description": "Preview a website or open a URL in the editor's Simple Browser. Useful for quickly viewing locally hosted websites, demos, or resources without leaving the coding environment.",
            "parameters": {
                "type": "object",
                "properties": {
                    "url": {
                        "type": "string",
                        "description": "The website URL to preview or open in the Simple Browser inside the editor. Must be either an http or https URL"
                    }
                },
                "required": [
                    "url"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "read_file",
            "description": "Read the contents of a file.\n\nYou must specify the line range you're interested in. Line numbers are 1-indexed. If the file contents returned are insufficient for your task, you may call this tool again to retrieve more content. Prefer reading larger ranges over doing many small reads.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "description": "The absolute path of the file to read.",
                        "type": "string"
                    },
                    "startLine": {
                        "type": "number",
                        "description": "The line number to start reading from, 1-based."
                    },
                    "endLine": {
                        "type": "number",
                        "description": "The inclusive line number to end reading at, 1-based."
                    }
                },
                "required": [
                    "filePath",
                    "startLine",
                    "endLine"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "read_notebook_cell_output",
            "description": "This tool will retrieve the output for a notebook cell from its most recent execution or restored from disk. The cell may have output even when it has not been run in the current kernel session. This tool has a higher token limit for output length than the runNotebookCell tool.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "An absolute path to the notebook file with the cell to run, or the URI of a untitled, not yet named, file, such as `untitled:Untitled-1.ipynb"
                    },
                    "cellId": {
                        "type": "string",
                        "description": "The ID of the cell for which output should be retrieved."
                    }
                },
                "required": [
                    "filePath",
                    "cellId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "replace_string_in_file",
            "description": "This is a tool for making edits in an existing file in the workspace. For moving or renaming files, use run in terminal tool with the 'mv' command instead. For larger edits, split them into smaller edits and call the edit tool multiple times to ensure accuracy. Before editing, always ensure you have the context to understand the file's contents and context. To edit a file, provide: 1) filePath (absolute path), 2) oldString (MUST be the exact literal text to replace including all whitespace, indentation, newlines, and surrounding code etc), and 3) newString (MUST be the exact literal text to replace \\`oldString\\` with (also including all whitespace, indentation, newlines, and surrounding code etc.). Ensure the resulting code is correct and idiomatic.). Each use of this tool replaces exactly ONE occurrence of oldString.\n\nCRITICAL for \\`oldString\\`: Must uniquely identify the single instance to change. Include at least 3 lines of context BEFORE and AFTER the target text, matching whitespace and indentation precisely. If this string matches multiple locations, or does not match exactly, the tool will fail. Never use 'Lines 123-456 omitted' from summarized documents or ...existing code... comments in the oldString or newString.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "An absolute path to the file to edit."
                    },
                    "oldString": {
                        "type": "string",
                        "description": "The exact literal text to replace, preferably unescaped. For single replacements (default), include at least 3 lines of context BEFORE and AFTER the target text, matching whitespace and indentation precisely. For multiple replacements, specify expected_replacements parameter. If this string is not the exact literal text (i.e. you escaped it) or does not match exactly, the tool will fail."
                    },
                    "newString": {
                        "type": "string",
                        "description": "The exact literal text to replace `old_string` with, preferably unescaped. Provide the EXACT text. Ensure the resulting code is correct and idiomatic."
                    }
                },
                "required": [
                    "filePath",
                    "oldString",
                    "newString"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "run_notebook_cell",
            "description": "This is a tool for running a code cell in a notebook file directly in the notebook editor. The output from the execution will be returned. Code cells should be run as they are added or edited when working through a problem to bring the kernel state up to date and ensure the code executes successfully. Code cells are ready to run and don't require any pre-processing. If asked to run the first cell in a notebook, you should run the first code cell since markdown cells cannot be executed. NOTE: Avoid executing Markdown cells or providing Markdown cell IDs, as Markdown cells cannot be  executed.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "An absolute path to the notebook file with the cell to run, or the URI of a untitled, not yet named, file, such as `untitled:Untitled-1.ipynb"
                    },
                    "reason": {
                        "type": "string",
                        "description": "An optional explanation of why the cell is being run. This will be shown to the user before the tool is run and is not necessary if it's self-explanatory."
                    },
                    "cellId": {
                        "type": "string",
                        "description": "The ID for the code cell to execute. Avoid providing markdown cell IDs as nothing will be executed."
                    },
                    "continueOnError": {
                        "type": "boolean",
                        "description": "Whether or not execution should continue for remaining cells if an error is encountered. Default to false unless instructed otherwise."
                    }
                },
                "required": [
                    "filePath",
                    "cellId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "run_vscode_command",
            "description": "Run a command in VS Code. Use this tool to run a command in Visual Studio Code as part of a new workspace creation process only.",
            "parameters": {
                "type": "object",
                "properties": {
                    "commandId": {
                        "type": "string",
                        "description": "The ID of the command to execute. This should be in the format <command>."
                    },
                    "name": {
                        "type": "string",
                        "description": "The name of the command to execute. This should be a clear and concise description of the command."
                    },
                    "args": {
                        "type": "array",
                        "description": "The arguments to pass to the command. This should be an array of strings.",
                        "items": {
                            "type": "string"
                        }
                    }
                },
                "required": [
                    "commandId",
                    "name"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "semantic_search",
            "description": "Run a natural language search for relevant code or documentation comments from the user's current workspace. Returns relevant code snippets from the user's current workspace if it is large, or the full contents of the workspace if it is small.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {
                        "type": "string",
                        "description": "The query to search the codebase for. Should contain all relevant context. Should ideally be text that might appear in the codebase, such as function names, variable names, or comments."
                    }
                },
                "required": [
                    "query"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "test_failure",
            "description": "Includes test failure information in the prompt."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "vscode_searchExtensions_internal",
            "description": "This is a tool for browsing Visual Studio Code Extensions Marketplace. It allows the model to search for extensions and retrieve detailed information about them. The model should use this tool whenever it needs to discover extensions or resolve information about known ones. To use the tool, the model has to provide the category of the extensions, relevant search keywords, or known extension IDs. Note that search results may include false positives, so reviewing and filtering is recommended.",
            "parameters": {
                "type": "object",
                "properties": {
                    "category": {
                        "type": "string",
                        "description": "The category of extensions to search for",
                        "enum": [
                            "AI",
                            "Azure",
                            "Chat",
                            "Data Science",
                            "Debuggers",
                            "Extension Packs",
                            "Education",
                            "Formatters",
                            "Keymaps",
                            "Language Packs",
                            "Linters",
                            "Machine Learning",
                            "Notebooks",
                            "Programming Languages",
                            "SCM Providers",
                            "Snippets",
                            "Testing",
                            "Themes",
                            "Visualization",
                            "Other"
                        ]
                    },
                    "keywords": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "description": "The keywords to search for"
                    },
                    "ids": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "description": "The ids of the extensions to search for"
                    }
                }
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "create_and_run_task",
            "description": "Creates and runs a build, run, or custom task for the workspace by generating or adding to a tasks.json file based on the project structure (such as package.json or README.md). If the user asks to build, run, launch and they have no tasks.json file, use this tool. If they ask to create or add a task, use this tool.",
            "parameters": {
                "type": "object",
                "properties": {
                    "workspaceFolder": {
                        "type": "string",
                        "description": "The absolute path of the workspace folder where the tasks.json file will be created."
                    },
                    "task": {
                        "type": "object",
                        "description": "The task to add to the new tasks.json file.",
                        "properties": {
                            "label": {
                                "type": "string",
                                "description": "The label of the task."
                            },
                            "type": {
                                "type": "string",
                                "description": "The type of the task. The only supported value is 'shell'.",
                                "enum": [
                                    "shell"
                                ]
                            },
                            "command": {
                                "type": "string",
                                "description": "The shell command to run for the task. Use this to specify commands for building or running the application."
                            },
                            "args": {
                                "type": "array",
                                "description": "The arguments to pass to the command.",
                                "items": {
                                    "type": "string"
                                }
                            },
                            "isBackground": {
                                "type": "boolean",
                                "description": "Whether the task runs in the background without blocking the UI or other tasks. Set to true for long-running processes like watch tasks or servers that should continue executing without requiring user attention. When false, the task will block the terminal until completion."
                            },
                            "problemMatcher": {
                                "type": "array",
                                "description": "The problem matcher to use to parse task output for errors and warnings. Can be a predefined matcher like '$tsc' (TypeScript), '$eslint - stylish', '$gcc', etc., or a custom pattern defined in tasks.json. This helps VS Code display errors in the Problems panel and enables quick navigation to error locations.",
                                "items": {
                                    "type": "string"
                                }
                            },
                            "group": {
                                "type": "string",
                                "description": "The group to which the task belongs."
                            }
                        },
                        "required": [
                            "label",
                            "type",
                            "command"
                        ]
                    }
                },
                "required": [
                    "task",
                    "workspaceFolder"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_terminal_output",
            "description": "Get the output of a terminal command previously started with run_in_terminal",
            "parameters": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string",
                        "description": "The ID of the terminal to check."
                    }
                },
                "required": [
                    "id"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "manage_todo_list",
            "description": "Manage a structured todo list to track progress and plan tasks throughout your coding session. Use this tool VERY frequently to ensure task visibility and proper planning.\n\nWhen to use this tool:\n- Complex multi-step work requiring planning and tracking\n- When user provides multiple tasks or requests (numbered/comma-separated)\n- After receiving new instructions that require multiple steps\n- BEFORE starting work on any todo (mark as in-progress)\n- IMMEDIATELY after completing each todo (mark completed individually)\n- When breaking down larger tasks into smaller actionable steps\n- To give users visibility into your progress and planning\n\nWhen NOT to use:\n- Single, trivial tasks that can be completed in one step\n- Purely conversational/informational requests\n- When just reading files or performing simple searches\n\nCRITICAL workflow:\n1. Plan tasks by writing todo list with specific, actionable items\n2. Mark ONE todo as in-progress before starting work\n3. Complete the work for that specific todo\n4. Mark that todo as completed IMMEDIATELY\n5. Move to next todo and repeat\n\nTodo states:\n- not-started: Todo not yet begun\n- in-progress: Currently working (limit ONE at a time)\n- completed: Finished successfully\n\nIMPORTANT: Mark todos completed as soon as they are done. Do not batch completions.",
            "parameters": {
                "type": "object",
                "properties": {
                    "todoList": {
                        "type": "array",
                        "description": "Complete array of all todo items. Must include ALL items - both existing and new.",
                        "items": {
                            "type": "object",
                            "properties": {
                                "id": {
                                    "type": "number",
                                    "description": "Unique identifier for the todo. Use sequential numbers starting from 1."
                                },
                                "title": {
                                    "type": "string",
                                    "description": "Concise action-oriented todo label (3-7 words). Displayed in UI."
                                },
                                "status": {
                                    "type": "string",
                                    "enum": [
                                        "not-started",
                                        "in-progress",
                                        "completed"
                                    ],
                                    "description": "not-started: Not begun | in-progress: Currently working (max 1) | completed: Fully finished with no blockers"
                                }
                            },
                            "required": [
                                "id",
                                "title",
                                "status"
                            ]
                        }
                    }
                },
                "required": [
                    "todoList"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "run_in_terminal",
            "description": "This tool allows you to execute shell commands in a persistent zsh terminal session, preserving environment variables, working directory, and other context across multiple commands.\n\nCommand Execution:\n- Use && to chain simple commands on one line\n- Prefer pipelines | over temporary files for data flow\n- Never create a sub-shell (eg. bash -c \"command\") unless explicitly asked\n\nDirectory Management:\n- Must use absolute paths to avoid navigation issues\n- Use $PWD for current directory references\n- Consider using pushd/popd for directory stack management\n- Supports directory shortcuts like ~ and -\n\nProgram Execution:\n- Supports Python, Node.js, and other executables\n- Install packages via package managers (brew, apt, etc.)\n- Use which or command -v to verify command availability\n\nBackground Processes:\n- For long-running tasks (e.g., servers), set isBackground=true\n- Returns a terminal ID for checking status and runtime later\n\nOutput Management:\n- Output is automatically truncated if longer than 60KB to prevent context overflow\n- Use head, tail, grep, awk to filter and limit output size\n- For pager commands, disable paging: git --no-pager or add | cat\n- Use wc -l to count lines before displaying large outputs\n\nBest Practices:\n- Quote variables: \"$var\" instead of $var to handle spaces\n- Use find with -exec or xargs for file operations\n- Be specific with commands to avoid excessive output\n- Avoid printing credentials unless absolutely required\n- Use type to check command type (builtin, function, alias)\n- Use jobs, fg, bg for job control\n- Use [[ ]] for conditional tests instead of [ ]\n- Prefer $() over backticks for command substitution\n- Use setopt errexit for strict error handling\n- Take advantage of zsh globbing features (**, extended globs)",
            "parameters": {
                "type": "object",
                "properties": {
                    "command": {
                        "type": "string",
                        "description": "The command to run in the terminal."
                    },
                    "explanation": {
                        "type": "string",
                        "description": "A one-sentence description of what the command does. This will be shown to the user before the command is run."
                    },
                    "isBackground": {
                        "type": "boolean",
                        "description": "Whether the command starts a background process. If true, the command will run in the background and you will not see the output. If false, the tool call will block on the command finishing, and then you will get the output. Examples of background processes: building in watch mode, starting a server. You can check the output of a background process later on by using get_terminal_output."
                    }
                },
                "required": [
                    "command",
                    "explanation",
                    "isBackground"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "runSubagent",
            "description": "Launch a new agent to handle complex, multi-step tasks autonomously. This tool is good at researching complex questions, searching for code, and executing multi-step tasks. When you are searching for a keyword or file and are not confident that you will find the right match in the first few tries, use this agent to perform the search for you.\n\n- Agents do not run async or in the background, you will wait for the agent's result.\n- When the agent is done, it will return a single message back to you. The result returned by the agent is not visible to the user. To show the user the result, you should send a text message back to the user with a concise summary of the result.\n- Each agent invocation is stateless. You will not be able to send additional messages to the agent, nor will the agent be able to communicate with you outside of its final report. Therefore, your prompt should contain a highly detailed task description for the agent to perform autonomously and you should specify exactly what information the agent should return back to you in its final and only message to you.\n- The agent's outputs should generally be trusted\n- Clearly tell the agent whether you expect it to write code or just to do research (search, file reads, web fetches, etc.), since it is not aware of the user's intent",
            "parameters": {
                "type": "object",
                "properties": {
                    "prompt": {
                        "type": "string",
                        "description": "A detailed description of the task for the agent to perform"
                    },
                    "description": {
                        "type": "string",
                        "description": "A short (3-5 word) description of the task"
                    }
                },
                "required": [
                    "prompt",
                    "description"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "terminal_last_command",
            "description": "Get the last command run in the active terminal."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "terminal_selection",
            "description": "Get the current selection in the active terminal."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "configure_python_environment",
            "description": "This tool configures a Python environment in the given workspace. ALWAYS Use this tool to set up the user's chosen environment and ALWAYS call this tool before using any other Python related tools or running any Python command in the terminal.",
            "parameters": {
                "type": "object",
                "properties": {
                    "resourcePath": {
                        "type": "string",
                        "description": "The path to the Python file or workspace for which a Python Environment needs to be configured."
                    }
                },
                "required": []
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_python_environment_details",
            "description": "This tool will retrieve the details of the Python Environment for the specified file or workspace. The details returned include the 1. Type of Python Environment (conda, venv, etec), 2. Version of Python, 3. List of all installed Python packages with their versions. ALWAYS call configure_python_environment before using this tool.",
            "parameters": {
                "type": "object",
                "properties": {
                    "resourcePath": {
                        "type": "string",
                        "description": "The path to the Python file or workspace to get the environment information for."
                    }
                },
                "required": []
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_python_executable_details",
            "description": "This tool will retrieve the details of the Python Environment for the specified file or workspace. ALWAYS use this tool before executing any Python command in the terminal. This tool returns the details of how to construct the fully qualified path and or command including details such as arguments required to run Python in a terminal. Note: Instead of executing `python --version` or `python -c 'import sys; print(sys.executable)'`, use this tool to get the Python executable path to replace the `python` command. E.g. instead of using `python -c 'import sys; print(sys.executable)'`, use this tool to build the command `conda run -n <env_name> -c 'import sys; print(sys.executable)'`. ALWAYS call configure_python_environment before using this tool.",
            "parameters": {
                "type": "object",
                "properties": {
                    "resourcePath": {
                        "type": "string",
                        "description": "The path to the Python file or workspace to get the executable information for. If not provided, the current workspace will be used. Where possible pass the path to the file or workspace."
                    }
                },
                "required": []
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "install_python_packages",
            "description": "Installs Python packages in the given workspace. Use this tool to install Python packages in the user's chosen Python environment. ALWAYS call configure_python_environment before using this tool.",
            "parameters": {
                "type": "object",
                "properties": {
                    "packageList": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "description": "The list of Python packages to install."
                    },
                    "resourcePath": {
                        "type": "string",
                        "description": "The path to the Python file or workspace into which the packages are installed. If not provided, the current workspace will be used. Where possible pass the path to the file or workspace."
                    }
                },
                "required": [
                    "packageList"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "container-tools_get-config",
            "description": "Gets the configuration the user has set for the container and container orchestrator (compose) CLIs, including the correct base commands to use and any environment variables that will be set. This tool **MUST** be called before generating or running any container or container orchestrator CLI commands, to ensure the correct base command is used."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_java_debugging_control_tools",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group of tools is focused on controlling the execution flow of Java programs during debugging sessions. The tools allow developers to step through code execution, set and remove breakpoints, and manage the debugging process effectively. The 'debug_step_operation' tool provides various commands to navigate through the code, such as stepping into methods, stepping out, stepping over lines, continuing execution to the next breakpoint, and pausing the program. This functionality is essential for closely examining how the code behaves at runtime and identifying issues in logic or flow.\n\nThe 'set_java_breakpoint' tool enables developers to strategically pause execution at specific lines in the code, allowing for inspection of the program state. It supports advanced features like conditional breakpoints and logpoints, which enhance the debugging experience by providing flexibility in when and how the program halts. This tool is best used in conjunction with the stepping operations to minimize the number of breakpoints while maximizing the effectiveness of the debugging process.\n\nConversely, the 'remove_java_breakpoints' tool helps maintain a clean debugging environment by allowing users to remove specific breakpoints or clear all breakpoints globally. This is particularly useful after completing an investigation or when preparing to set new breakpoints, ensuring that the debugging session remains focused and efficient. Best practices suggest keeping only a few active breakpoints at any time to avoid clutter and confusion.\n\nTogether, these tools create a robust framework for debugging Java applications, enabling developers to control execution flow, inspect program states, and manage breakpoints effectively. By using these tools in tandem, developers can streamline their debugging process, making it easier to identify and resolve issues in their code.\n\nOverall, this group is essential for any Java developer looking to enhance their debugging capabilities and improve code quality through effective inspection and control of program execution.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_java_debug_session_management_tools",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group of tools is designed to manage and gather information about active Java debug sessions. The tools provide essential functionalities for monitoring the state of the debugging process, inspecting threads, and terminating sessions when necessary. The 'get_debug_session_info' tool is crucial for obtaining details about the current debug session, including its status (paused or running), session ID, and available actions based on the current state. This information is vital for developers to understand what operations can be performed at any given time during debugging.\n\nThe 'get_debug_threads' tool complements the session information by listing all threads within the debugged application, detailing their IDs, names, and states. This allows developers to focus on specific threads that are suspended, enabling them to inspect variables or evaluate expressions only for those threads. This targeted approach is essential for diagnosing issues that may be thread-specific, particularly in multi-threaded applications.\n\nThe 'stop_debug_session' tool provides a straightforward way to terminate an active debug session once the investigation is complete or when a restart is needed. This tool not only ends the debugging process but also helps in cleaning up resources associated with the session. Developers can optionally provide a reason for stopping the session, which can be useful for documentation or future reference.\n\nTogether, these tools form a comprehensive suite for managing Java debug sessions, allowing developers to monitor session status, inspect thread activity, and cleanly terminate sessions. By utilizing these tools, developers can ensure that their debugging efforts are organized and efficient, leading to quicker identification and resolution of issues.\n\nIn summary, this group is essential for Java developers who need to maintain control over their debugging sessions, providing the necessary tools to gather information, manage threads, and conclude sessions effectively.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "debug_java_application",
            "description": "Launch or attach to a Java application in debug mode with automatic compilation and classpath resolution. The tool handles building the project, resolving dependencies, starting the JVM with JDWP enabled, and auto-attaching the VS Code debugger. Use this as the first step to establish a debug session. The debug process runs in the background until stopped. Example usage: Debug a main class ('com.example.Main'), a JAR file ('target/app.jar'), or with program arguments (['--port=8080']).",
            "parameters": {
                "type": "object",
                "properties": {
                    "target": {
                        "type": "string",
                        "description": "What to debug: 1) Main class name - simple ('App') or fully qualified ('com.example.Main'). Tool auto-detects package from source files. 2) JAR file path ('target/app.jar'). 3) Raw Java command arguments ('-cp bin com.example.Main'). The tool automatically finds the .class file for simple class names."
                    },
                    "workspacePath": {
                        "type": "string",
                        "description": "Absolute path to the Java project root directory containing pom.xml, build.gradle, or .java source files. This is the working directory for compilation and debugging."
                    },
                    "args": {
                        "type": "array",
                        "items": {
                            "type": "string"
                        },
                        "description": "Optional command-line arguments to pass to the Java main method (e.g., ['arg1', 'arg2', '--flag=value']). These are program arguments, not JVM arguments."
                    },
                    "skipBuild": {
                        "type": "boolean",
                        "description": "Whether to skip compilation before debugging. DEFAULT: false (tool will automatically compile the project). Set to true only when you have already compiled the project and want to use an explicit classpath. In most cases, leave this as false to let the tool handle compilation automatically.",
                        "default": false
                    },
                    "classpath": {
                        "type": "string",
                        "description": "Explicit classpath to use for debugging. REQUIRED when skipBuild is true. Format: absolute paths separated by system path delimiter (';' on Windows, ':' on Unix). Example: 'C:\\project\\target\\classes;C:\\project\\lib\\dep.jar' or '/project/target/classes:/project/lib/dep.jar'. If not provided and skipBuild is false, the tool will automatically resolve the classpath."
                    },
                    "waitForSession": {
                        "type": "boolean",
                        "description": "Whether to wait for the debug session to start before returning. DEFAULT: false (returns immediately after sending debug command). Set to true to wait up to 30 seconds for VS Code to confirm the debug session has started and is ready. Useful when you need to ensure the debugger is attached before proceeding with breakpoint operations.",
                        "default": false
                    }
                },
                "required": [
                    "target",
                    "workspacePath"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "evaluate_debug_expression",
            "description": "Evaluate a Java expression in a specific thread's debug context. Access local variables, parameters, fields, and invoke methods. Returns the result with type information. REQUIRES: Active debug session with at least one SUSPENDED thread. For multi-threaded debugging, use threadId to specify which thread's context to use. If no threadId is provided, uses the first suspended thread. Examples: 'user.getName()', 'list.size() > 10', 'counter == null'.",
            "parameters": {
                "type": "object",
                "properties": {
                    "expression": {
                        "type": "string",
                        "description": "The Java expression to evaluate. Can be a variable name, field access, method call, or complex expression. Example: 'user.age', 'calculateTotal()', 'count > 0 && !items.isEmpty()'."
                    },
                    "threadId": {
                        "type": "number",
                        "description": "Thread ID for evaluation context. Use get_debug_threads() to list available threads with their IDs and states. Only SUSPENDED threads can evaluate expressions. If omitted, uses the first suspended thread found."
                    },
                    "frameId": {
                        "type": "number",
                        "description": "Optional stack frame ID for evaluation context. Default: 0 (current frame). Variables and methods from the specified frame will be accessible.",
                        "default": 0
                    },
                    "context": {
                        "type": "string",
                        "enum": [
                            "watch",
                            "repl",
                            "hover"
                        ],
                        "description": "Evaluation context: 'watch' - for watch expressions, 'repl' - for debug console input, 'hover' - for hover tooltips. Affects how side effects are handled. Default: 'repl'.",
                        "default": "repl"
                    }
                },
                "required": [
                    "expression"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_debug_stack_trace",
            "description": "Retrieve the call stack showing all method calls leading to the current execution point. Returns method names, source files, and line numbers for each frame. REQUIRES: Active debug session in paused state. Essential for understanding program flow, tracing how code was reached, and identifying unexpected execution paths.",
            "parameters": {
                "type": "object",
                "properties": {
                    "threadId": {
                        "type": "number",
                        "description": "Optional thread ID. If not specified, uses the currently selected thread. Use get_debug_threads to list available threads."
                    },
                    "maxDepth": {
                        "type": "number",
                        "description": "Maximum number of stack frames to retrieve. Default: 50. Use smaller values for shallow inspection, larger for deep call stacks.",
                        "default": 50
                    }
                },
                "required": []
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "get_debug_variables",
            "description": "Inspect variables in a specific thread's stack frame: local variables, method parameters, static fields, and instance fields. Returns variable names, types, and values. Supports filtering by scope type or name pattern. REQUIRES: Active debug session with at least one SUSPENDED thread. For multi-threaded debugging, use threadId to specify which thread's variables to inspect. If no threadId is provided, uses the first suspended thread.",
            "parameters": {
                "type": "object",
                "properties": {
                    "threadId": {
                        "type": "number",
                        "description": "Thread ID to inspect. Use get_debug_threads() to list available threads with their IDs and states. Only SUSPENDED threads can be inspected. If omitted, uses the first suspended thread found."
                    },
                    "frameId": {
                        "type": "number",
                        "description": "Optional stack frame ID. Default is 0 (current/top frame). Use get_debug_stack_trace to get available frame IDs. Higher numbers are deeper in the call stack."
                    },
                    "scopeType": {
                        "type": "string",
                        "enum": [
                            "local",
                            "static",
                            "all"
                        ],
                        "description": "Type of variables to retrieve: 'local' - only local variables and parameters, 'static' - only static class variables, 'all' - both local and static. Default: 'all'."
                    },
                    "filter": {
                        "type": "string",
                        "description": "Optional filter pattern to match variable names. Supports wildcards (*). Example: 'user*' matches 'userName', 'userId'. Leave empty to get all variables."
                    }
                },
                "required": []
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_add_or_commit",
            "description": "Add file contents to the index (git add <pathspec>) OR record changes to the repository (git commit -m <message> [files...]). Use the 'action' parameter to specify which action to perform.",
            "parameters": {
                "type": "object",
                "properties": {
                    "action": {
                        "description": "The action to perform: 'add' or 'commit'",
                        "enum": [
                            "add",
                            "commit"
                        ],
                        "type": "string"
                    },
                    "directory": {
                        "description": "The directory to run git add or commit in",
                        "type": "string"
                    },
                    "files": {
                        "description": "Optional array of files to add or commit. If omitted, all files are added or all staged changes are committed.",
                        "items": {
                            "type": "string"
                        },
                        "type": "array"
                    },
                    "message": {
                        "description": "The commit message (required if action is 'commit')",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "action"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_blame",
            "description": "Show what revision and author last modified each line of a file (git blame <file>).",
            "parameters": {
                "type": "object",
                "properties": {
                    "directory": {
                        "description": "The directory to run git blame in",
                        "type": "string"
                    },
                    "file": {
                        "description": "The file to blame",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "file"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_branch",
            "description": "List or create branches (git branch).",
            "parameters": {
                "type": "object",
                "properties": {
                    "action": {
                        "description": "Git branch action to be executed",
                        "enum": [
                            "create",
                            "list"
                        ],
                        "type": "string"
                    },
                    "branch_name": {
                        "description": "(Optional) Name of the branch to create or delete",
                        "type": "string"
                    },
                    "directory": {
                        "description": "The directory to run git branch in",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "action"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_checkout",
            "description": "Switch branches or restore working tree files (git checkout <branch>).",
            "parameters": {
                "type": "object",
                "properties": {
                    "branch": {
                        "description": "The branch to checkout. This must be a valid branch name without spaces",
                        "type": "string"
                    },
                    "directory": {
                        "description": "The directory to run git checkout in",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "branch"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_log_or_diff",
            "description": "Show commit logs or changes between commits (git log --oneline or git diff).",
            "parameters": {
                "type": "object",
                "properties": {
                    "action": {
                        "description": "The action to perform: 'log' for commit logs or 'diff' for changes",
                        "enum": [
                            "log",
                            "diff"
                        ],
                        "type": "string"
                    },
                    "commit": {
                        "description": "Optional commit to compare against HEAD for 'diff', defaults to HEAD",
                        "type": "string"
                    },
                    "directory": {
                        "description": "The directory to run the command in",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "action"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_push",
            "description": "Update remote refs along with associated objects (git push).",
            "parameters": {
                "type": "object",
                "properties": {
                    "directory": {
                        "description": "The directory to run git push in",
                        "type": "string"
                    }
                },
                "required": [
                    "directory"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_stash",
            "description": "Stash the changes in a dirty working directory (git stash).",
            "parameters": {
                "type": "object",
                "properties": {
                    "directory": {
                        "description": "The directory to run git stash in",
                        "type": "string"
                    },
                    "name": {
                        "description": "Optional name for the stash (used as the stash message)",
                        "type": "string"
                    }
                },
                "required": [
                    "directory"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_status",
            "description": "Show the working tree status (git status).",
            "parameters": {
                "type": "object",
                "properties": {
                    "directory": {
                        "description": "The directory to run git status in",
                        "type": "string"
                    }
                },
                "required": [
                    "directory"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_git_worktree",
            "description": "List or add git worktrees (git worktree <action>).",
            "parameters": {
                "type": "object",
                "properties": {
                    "action": {
                        "description": "Git worktree action to be executed",
                        "enum": [
                            "list",
                            "add"
                        ],
                        "type": "string"
                    },
                    "branch": {
                        "description": "(Optional) Existing branch for the new worktree (used for add)",
                        "type": "string"
                    },
                    "directory": {
                        "description": "The directory to run git worktree in",
                        "type": "string"
                    },
                    "path": {
                        "description": "(Optional) Path for the worktree (required for add)",
                        "type": "string"
                    }
                },
                "required": [
                    "directory",
                    "action"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_gitkraken_workspace_list",
            "description": " Lists all Gitkraken workspaces",
            "parameters": {
                "type": "object",
                "properties": {}
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_issues_add_comment",
            "description": "Add a comment to an issue",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_organization": {
                        "description": "Optionally set the Azure DevOps organization name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "comment": {
                        "description": "The text content of the comment",
                        "type": "string"
                    },
                    "issue_id": {
                        "description": "The ID of the issue to comment on",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the issue provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "jira",
                            "azure",
                            "linear"
                        ],
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Repository name. This is required for GitHub and GitLab",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Organization name. This is required for GitHub and GitLab",
                        "type": "string"
                    }
                },
                "required": [
                    "provider",
                    "issue_id",
                    "comment"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_issues_assigned_to_me",
            "description": "Fetch issues assigned to the user",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_organization": {
                        "description": "Optionally set the Azure DevOps organization name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "page": {
                        "description": "Optional parameter to specify the page number, defaults to 1",
                        "type": "number"
                    },
                    "provider": {
                        "description": "Specify the issue provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "jira",
                            "azure",
                            "linear"
                        ],
                        "type": "string"
                    }
                },
                "required": [
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_issues_get_detail",
            "description": "Retrieve detailed information about a specific issue by its unique ID",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_organization": {
                        "description": "Optionally set the Azure DevOps organization name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name. Required for Azure DevOps",
                        "type": "string"
                    },
                    "issue_id": {
                        "description": "The ID of the issue to retrieve",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the issue provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "jira",
                            "azure",
                            "linear"
                        ],
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Repository name. This is required for GitHub and GitLab",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Organization name. This is required for GitHub and GitLab",
                        "type": "string"
                    }
                },
                "required": [
                    "provider",
                    "issue_id"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_pull_request_assigned_to_me",
            "description": "Search pull requests where you are the assignee, author, or reviewer",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "is_closed": {
                        "description": "Set to true if you want to search for closed pull requests",
                        "type": "boolean"
                    },
                    "page": {
                        "description": "Optional parameter to specify the page number, defaults to 1",
                        "type": "number"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    }
                },
                "required": [
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_pull_request_create",
            "description": "Create a new pull request",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "body": {
                        "description": "The body/description of the pull request",
                        "type": "string"
                    },
                    "is_draft": {
                        "description": "Create as draft pull request",
                        "type": "boolean"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "source_branch": {
                        "description": "Source branch from which the pull request will be created",
                        "type": "string"
                    },
                    "target_branch": {
                        "description": "Target branch where the pull request will be merged",
                        "type": "string"
                    },
                    "title": {
                        "description": "The title of the pull request",
                        "type": "string"
                    }
                },
                "required": [
                    "repository_name",
                    "repository_organization",
                    "title",
                    "source_branch",
                    "target_branch",
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_pull_request_create_review",
            "description": "Create a review for a pull request",
            "parameters": {
                "type": "object",
                "properties": {
                    "approve": {
                        "description": "Set to true if you want to approve the pull request",
                        "type": "boolean"
                    },
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "pull_request_id": {
                        "description": "ID of the pull request to create the review for",
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "review": {
                        "description": "Comment to add to the pull request review",
                        "type": "string"
                    }
                },
                "required": [
                    "repository_name",
                    "repository_organization",
                    "pull_request_id",
                    "review",
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_pull_request_get_comments",
            "description": "Get all the comments in a pull requests",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "pull_request_id": {
                        "description": "ID of the pull request to add the comment to",
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request",
                        "type": "string"
                    }
                },
                "required": [
                    "repository_name",
                    "repository_organization",
                    "pull_request_id",
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_pull_request_get_detail",
            "description": "Get an specific pull request",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "pull_request_files": {
                        "description": "Set to true if you want to retrieve the files changed in the pull request. Not supported by Azure DevOps.",
                        "type": "boolean"
                    },
                    "pull_request_id": {
                        "description": "ID of the pull request to retrieve",
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request",
                        "type": "string"
                    }
                },
                "required": [
                    "pull_request_id",
                    "repository_name",
                    "repository_organization",
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_gitkraken_repository_get_file_content",
            "description": "Get file content from a repository",
            "parameters": {
                "type": "object",
                "properties": {
                    "azure_project": {
                        "description": "Optionally set the Azure DevOps project name of the pull request. Required for Azure DevOps",
                        "type": "string"
                    },
                    "file_path": {
                        "description": "File path to retrieve from the repository",
                        "type": "string"
                    },
                    "provider": {
                        "description": "Specify the git provider",
                        "enum": [
                            "github",
                            "gitlab",
                            "bitbucket",
                            "azure"
                        ],
                        "type": "string"
                    },
                    "ref": {
                        "description": "Set the branch, tag, or commit SHA to retrieve the file from",
                        "type": "string"
                    },
                    "repository_name": {
                        "description": "Set the repository name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    },
                    "repository_organization": {
                        "description": "Set the organization name of the pull request. Required for Azure DevOps and Bitbucket",
                        "type": "string"
                    }
                },
                "required": [
                    "repository_name",
                    "repository_organization",
                    "ref",
                    "file_path",
                    "provider"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_python_syntax_validation_and_execution",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group of tools focuses on validating and executing Python code snippets within a workspace environment. The 'Check Python file for syntax errors' tool allows users to identify syntax issues in Python files, providing detailed error messages that include line numbers and types of errors. This is particularly useful for debugging and validating code before execution. The 'Execute Python code snippets directly in the workspace environment' tool enables users to run Python code snippets seamlessly, avoiding common terminal execution issues such as shell escaping. It provides clean output and is ideal for quick testing and validation of code. Additionally, the 'Validate Python code snippets for syntax errors without saving to file' tool allows for on-the-fly syntax checking of code snippets, making it easier to catch errors before running the code. Together, these tools streamline the process of writing, validating, and executing Python code, enhancing productivity and reducing errors.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_python_import_analysis_tools",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group is dedicated to analyzing and managing Python imports within a workspace. The 'Analyze imports across workspace user files' tool provides insights into the top-level modules imported in user files, helping developers identify missing dependencies and understand the project's import patterns. The 'Get available top-level modules from installed Python packages in environment' tool complements this by listing all the modules that can be imported from installed packages, allowing users to verify the availability of necessary libraries. Together, these tools facilitate dependency management and help users ensure that their code can access the required modules, ultimately leading to smoother development processes and fewer runtime errors related to missing imports.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_python_environment_management",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group of tools is focused on managing Python environments within a workspace. The 'Get Python environment information for workspace' tool provides users with details about the current active environment and all available environments, which is essential for troubleshooting environment-related issues. The 'Get current Python analysis settings and configuration for a workspace' tool allows users to review their Python analysis settings, helping them diagnose any configuration problems that may arise. Additionally, the 'Switch active Python environment for workspace' tool enables users to easily change their active Python environment, whether switching between different Python versions or virtual environments. Together, these tools empower developers to effectively manage their Python environments, ensuring that their projects run smoothly and are configured correctly.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_workspace_structure_and_file_management",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group provides tools for understanding and managing the structure of a Python workspace. The 'Get workspace root directories' tool allows users to identify the root directories of their workspace, which is crucial for navigating and organizing project files. The 'Get list of all user Python files in workspace' tool complements this by providing a comprehensive list of user-created Python files, excluding library and dependency files. This is particularly useful for analyzing user code and searching through project files. Together, these tools enhance the user's ability to manage their workspace effectively, making it easier to locate files, understand project organization, and perform operations on user-created Python scripts.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_pylance_mcp_s_pylanceDocuments",
            "description": "Search Pylance documentation for Python language server help, configuration guidance, feature explanations, and troubleshooting. Returns comprehensive answers about Pylance settings, capabilities, and usage. Use when users ask: How to configure Pylance? What features are available? How to fix Pylance issues?",
            "parameters": {
                "type": "object",
                "properties": {
                    "search": {
                        "type": "string",
                        "description": "Detailed question in natural language. Think of it as a prompt for an LLM. Do not use keyword search terms."
                    }
                },
                "required": [
                    "search"
                ],
                "additionalProperties": false,
                "$schema": "http://json-schema.org/draft-07/schema#"
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mcp_pylance_mcp_s_pylanceInvokeRefactoring",
            "description": "Apply automated code refactoring to Python files. Returns refactored content (does not modify original file) unless mode is \"update\". Use for: extracting functions, organizing imports, improving code structure, applying refactoring patterns.  Optional \"mode\" parameter: \"update\" updates the file, \"edits\" returns a WorkspaceEdit, \"string\" returns updated content as string. If mode is not specified, \"update\" will be used as the default. The \"edits\" mode is helpful for determining if a file needs changes (for example, to remove unused imports or fix import formatting) without making any modifications; if no changes are needed, the result will be either an empty WorkspaceEdit or a message indicating that no text edits were found. Available refactorings: source.unusedImports: - Removes all unused import statements from a Python file. Use when imports are imported but never referenced in the code. Requires fileUri parameter pointing to a Python file with unused imports.\nsource.convertImportFormat: - Converts import statements between absolute and relative formats according to python.analysis.importFormat setting. Use when import format consistency is needed. Requires fileUri parameter pointing to a Python file with imports to convert.\nsource.convertImportStar: - Converts all wildcard imports (from module import *) to explicit imports listing all imported symbols. Use when explicit imports are preferred for better code clarity and IDE support. Requires fileUri parameter pointing to a Python file with wildcard imports.\nsource.addTypeAnnotation: - Adds type annotations to all variables and functions in a Python file that can be inferred from their usage. Use when type hints are needed for better type checking and code clarity. Requires fileUri parameter pointing to a Python file with unannotated variables or functions.\nsource.fixAll.pylance: - Applies all available automatic code fixes from python.analysis.fixAll setting. Use when multiple code issues need to be addressed simultaneously. Requires fileUri parameter pointing to a Python file with fixable issues.",
            "parameters": {
                "type": "object",
                "properties": {
                    "fileUri": {
                        "type": "string",
                        "description": "The uri of the file to invoke the refactoring."
                    },
                    "name": {
                        "type": "string",
                        "description": "The name of the refactoring to invoke. This must be one of these [source.unusedImports, source.convertImportFormat, source.convertImportStar, source.addTypeAnnotation, source.fixAll.pylance]"
                    },
                    "mode": {
                        "type": "string",
                        "enum": [
                            "update",
                            "edits",
                            "string"
                        ],
                        "description": "Determines the output mode: \"update\" updates the file directly, \"edits\" returns a WorkspaceEdit, \"string\" returns the updated content as a string. If omitted, \"update\" will be used as the default. The \"edits\" mode is especially useful for checking if any changes are needed (such as unused imports or import formatting issues) without modifying the file, as it will return a WorkspaceEdit only if edits are required."
                    }
                },
                "required": [
                    "fileUri",
                    "name"
                ],
                "additionalProperties": false,
                "$schema": "http://json-schema.org/draft-07/schema#"
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "activate_mssql_database_exploration_tools",
            "description": "Call this tool when you need access to a new category of tools. The category of tools is described as follows:\n\nThis group of tools is designed for exploring and retrieving metadata from Microsoft SQL Server (MSSQL) databases. Each tool serves a specific purpose in listing various database components, making it easier for database administrators and developers to understand the structure and contents of their databases. The tools included in this group allow users to list databases, functions, schemas, tables, and views within a connected MSSQL server, providing a comprehensive overview of the database environment.\n\nThe 'mssql_list_databases' tool enables users to retrieve a list of all available databases on the connected MSSQL server, which is essential for identifying the databases that can be worked with. Following this, the 'mssql_list_schemas' tool allows users to delve deeper into a specific database by listing all schemas, which are crucial for organizing database objects and managing permissions.\n\nTo further explore the contents of a database, the 'mssql_list_tables' and 'mssql_list_views' tools provide lists of tables and views, respectively. Tables are the primary storage structure for data, while views offer a way to present data in a specific format or structure without altering the underlying tables. Additionally, the 'mssql_list_functions' tool allows users to identify all functions defined within a database, which can be critical for understanding the logic and operations that can be performed on the data.\n\nTogether, these tools facilitate a thorough exploration of the database schema and its components, enabling users to efficiently manage and interact with their MSSQL databases. They can be used sequentially to gather a complete picture of the database structure, making them invaluable for tasks such as database auditing, documentation, and development.\n\nOverall, the MSSQL database exploration tools empower users to navigate and understand their database environments effectively, ensuring they can make informed decisions regarding database management and development.\n\nBe sure to call this tool if you need a capability related to the above."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_change_database",
            "description": "Change the database for an existing MSSQL connection. Before changing, consider using mssql_list_databases to show available database options to the user. Disconnects from current database and reconnects to the specified database using the same connection credentials.",
            "parameters": {
                "type": "object",
                "properties": {
                    "connectionId": {
                        "description": "Connection ID to change database for.",
                        "title": "Connection ID",
                        "type": "string"
                    },
                    "database": {
                        "description": "Database name to switch to.",
                        "title": "Database Name",
                        "type": "string"
                    }
                },
                "required": [
                    "connectionId",
                    "database"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_connect",
            "description": "Connect to an MSSQL database server using a server name, an optional database name, and an optional profileId. The server name must be retrieved from mssql_list_servers. The profileId should be used ONLY when the user explicitly mentions a profile name, profile ID, or wants to connect 'using profile X'. Returns a connection ID that is used to interact with the database with other mssql tools. If a specific database is given and the connection fails, use mssql_list_databases against a connection to the default database to find the correct database name. The connection ID is a UUID.",
            "parameters": {
                "type": "object",
                "properties": {
                    "serverName": {
                        "description": "Server name to connect to. Should be validated with mssql_list_servers.",
                        "title": "Server Name",
                        "type": "string"
                    },
                    "database": {
                        "anyOf": [
                            {
                                "type": "string"
                            },
                            {
                                "type": "null"
                            }
                        ],
                        "default": null,
                        "description": "Optional database name to connect to. If omitted, uses the server's default database.",
                        "title": "Database Name"
                    },
                    "profileId": {
                        "description": "ID of a saved connection profile to use for connecting. Use ONLY when the user explicitly mentions a profile name, profile ID, or wants to connect 'using profile X'.",
                        "title": "Profile ID",
                        "anyOf": [
                            {
                                "type": "string"
                            },
                            {
                                "type": "null"
                            }
                        ]
                    }
                },
                "required": [
                    "serverName"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_disconnect",
            "description": "Disconnect from a server or specific database. Returns a success message.",
            "parameters": {
                "type": "object",
                "properties": {
                    "connectionId": {
                        "description": "Connection ID to disconnect.",
                        "title": "Connection ID",
                        "type": "string"
                    }
                },
                "required": [
                    "connectionId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_get_connection_details",
            "description": "Get connection details for a specific connection ID. Returns connection information including server, database, authentication type, and user details.",
            "parameters": {
                "type": "object",
                "properties": {
                    "connectionId": {
                        "description": "Connection ID to get details for.",
                        "title": "Connection ID",
                        "type": "string"
                    }
                },
                "required": [
                    "connectionId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_list_servers",
            "description": "List all available MSSQL servers. Returns a list of server names."
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_run_query",
            "description": "Execute a SQL query against a connected MSSQL database. Returns query results including row count, column information, and data rows. Use this tool to run SELECT, INSERT, UPDATE, DELETE, or other SQL statements. IMPORTANT: This tool will execute ANY SQL statement provided - be extremely careful with write operations (INSERT, UPDATE, DELETE, CREATE, ALTER, DROP) as they will modify or destroy data. Always verify the query is safe before execution, especially for operations without WHERE clauses or that affect system objects.",
            "parameters": {
                "type": "object",
                "properties": {
                    "connectionId": {
                        "description": "Connection ID to execute the query against.",
                        "title": "Connection ID",
                        "type": "string"
                    },
                    "query": {
                        "description": "SQL query to execute.",
                        "title": "SQL Query",
                        "type": "string"
                    },
                    "queryTypes": {
                        "description": "Classification of SQL operation types present in the query. Used for telemetry to understand tool usage patterns without capturing user content. Analyze the query and select all operation types that apply.",
                        "title": "Query Types",
                        "type": "array",
                        "items": {
                            "type": "string",
                            "enum": [
                                "SELECT",
                                "INSERT",
                                "UPDATE",
                                "DELETE",
                                "CREATE",
                                "ALTER",
                                "DROP",
                                "TRUNCATE",
                                "MERGE",
                                "JOIN",
                                "CTE",
                                "STORED_PROCEDURE",
                                "FUNCTION",
                                "VIEW",
                                "INDEX",
                                "TRANSACTION",
                                "GRANT",
                                "REVOKE",
                                "BACKUP",
                                "RESTORE",
                                "EXEC",
                                "DECLARE",
                                "IF",
                                "WHILE",
                                "TRY_CATCH",
                                "TEMP_TABLE",
                                "CONSTRAINT",
                                "TRIGGER",
                                "SET",
                                "OTHER"
                            ]
                        }
                    },
                    "queryIntent": {
                        "description": "Primary use case or scenario that best describes what the user is trying to accomplish with this query. Used for telemetry to understand user workflows without capturing user content.",
                        "title": "Query Intent",
                        "type": "string",
                        "enum": [
                            "data_exploration",
                            "data_analysis",
                            "data_migration",
                            "troubleshooting",
                            "schema_creation",
                            "schema_modification",
                            "schema_exploration",
                            "data_maintenance",
                            "data_seeding",
                            "testing_validation",
                            "backup_restore",
                            "performance_tuning",
                            "learning_education",
                            "other"
                        ]
                    }
                },
                "required": [
                    "connectionId",
                    "query",
                    "queryTypes",
                    "queryIntent"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "mssql_show_schema",
            "description": "Open an interactive schema designer for a MSSQL database. This tool takes a connection ID as input and opens a graphical view of the database schema, including tables and relationships.",
            "parameters": {
                "type": "object",
                "properties": {
                    "connectionId": {
                        "description": "Connection ID to use for schema visualization.",
                        "title": "Connection ID",
                        "type": "string"
                    }
                },
                "required": [
                    "connectionId"
                ]
            }
        },
        "type": "function"
    },
    {
        "function": {
            "name": "remove_java_breakpoints",
            "description": "Remove breakpoints: specific breakpoint by file and line, all breakpoints in a file, or all breakpoints globally. Use this to clean up after investigation or before setting new breakpoints. Best practice: keep only 1-2 active breakpoints at a time; remove old ones before adding new ones.",
            "parameters": {
                "type": "object",
                "properties": {
                    "filePath": {
                        "type": "string",
                        "description": "Absolute path to the Java source file. If not provided, removes all breakpoints from all files."
                    },
                    "lineNumber": {
                        "type": "number",
                        "description": "Optional line number. If provided, removes only the breakpoint at this line. If omitted, removes all breakpoints in the specified file."
                    }
                },
                "required": []
            }
        },
        "type": "function"
    }
]
~~~
## Request Messages
### System
~~~md
You are an expert AI programming assistant, working with a user in the VS Code editor.
When asked for your name, you must respond with "GitHub Copilot". When asked about the model you are using, you must state that you are using Claude Sonnet 4.5.
Follow the user's requirements carefully & to the letter.
Follow Microsoft content policies.
Avoid content that violates copyrights.
If you are asked to generate content that is harmful, hateful, racist, sexist, lewd, or violent, only respond with "Sorry, I can't assist with that."
Keep your answers short and impersonal.
<instructions>
You are a highly sophisticated automated coding agent with expert-level knowledge across many different programming languages and frameworks and software engineering tasks - this encompasses debugging issues, implementing new features, restructuring code, and providing code explanations, among other engineering activities.
The user will ask a question, or ask you to perform a task, and it may require lots of research to answer correctly. There is a selection of tools that let you perform actions or retrieve helpful context to answer the user's question.
By default, implement changes rather than only suggesting them. If the user's intent is unclear, infer the most useful likely action and proceed with using tools to discover any missing details instead of guessing. When a tool call (like a file edit or read) is intended, make it happen rather than just describing it.
You can call tools repeatedly to take actions or gather as much context as needed until you have completed the task fully. Don't give up unless you are sure the request cannot be fulfilled with the tools you have. It's YOUR RESPONSIBILITY to make sure that you have done all you can to collect necessary context.
Continue working until the user's request is completely resolved before ending your turn and yielding back to the user. Only terminate your turn when you are certain the task is complete. Do not stop or hand back to the user when you encounter uncertainty — research or deduce the most reasonable approach and continue.

</instructions>
<workflowGuidance>
For complex projects that take multiple steps to complete, maintain careful tracking of what you're doing to ensure steady progress. Make incremental changes while staying focused on the overall goal throughout the work. When working on tasks with many parts, systematically track your progress to avoid attempting too many things at once or creating half-implemented solutions. Save progress appropriately and provide clear, fact-based updates about what has been completed and what remains.

When working on multi-step tasks, combine independent read-only operations in parallel batches when appropriate. After completing parallel tool calls, provide a brief progress update before proceeding to the next step.
For context gathering, parallelize discovery efficiently - launch varied queries together, read results, and deduplicate paths. Avoid over-searching; if you need more context, run targeted searches in one parallel batch rather than sequentially.
Get enough context quickly to act, then proceed with implementation. Balance thorough understanding with forward momentum.

<taskTracking>
Utilize the manage_todo_list tool extensively to organize work and provide visibility into your progress. This is essential for planning and ensures important steps aren't forgotten.

Break complex work into logical, actionable steps that can be tracked and verified. Update task status consistently throughout execution using the manage_todo_list tool:
- Mark tasks as in-progress when you begin working on them
- Mark tasks as completed immediately after finishing each one - do not batch completions

Task tracking is valuable for:
- Multi-step work requiring careful sequencing
- Breaking down ambiguous or complex requests
- Maintaining checkpoints for feedback and validation
- When users provide multiple requests or numbered tasks

Skip task tracking for simple, single-step operations that can be completed directly without additional planning.

</taskTracking>

</workflowGuidance>
<toolUseInstructions>
If the user is requesting a code sample, you can answer it directly without using any tools.
When using a tool, follow the JSON schema very carefully and make sure to include ALL required properties.
No need to ask permission before using a tool.
NEVER say the name of a tool to a user. For example, instead of saying that you'll use the run_in_terminal tool, say "I'll run the command in a terminal".
If you think running multiple tools can answer the user's question, prefer calling them in parallel whenever possible, but do not call semantic_search in parallel.
When using the read_file tool, prefer reading a large section over calling the read_file tool many times in sequence. You can also think of all the pieces you may be interested in and read them in parallel. Read large enough context to ensure you get what you need.
If semantic_search returns the full contents of the text files in the workspace, you have all the workspace context.
You can use the grep_search to get an overview of a file by searching for a string within that one file, instead of using read_file many times.
If you don't know exactly the string or filename pattern you're looking for, use semantic_search to do a semantic search across the workspace.
Don't call the run_in_terminal tool multiple times in parallel. Instead, run one command and wait for the output before running the next command.
When creating files, be intentional and avoid calling the create_file tool unnecessarily. Only create files that are essential to completing the user's request. 
When invoking a tool that takes a file path, always use the absolute file path. If the file has a scheme like untitled: or vscode-userdata:, then use a URI with the scheme.
NEVER try to edit a file by running terminal commands unless the user specifically asks for it.
Tools can be disabled by the user. You may see tools used previously in the conversation that are not currently available. Be careful to only use the tools that are currently available to you.

</toolUseInstructions>
<communicationStyle>
Maintain clarity and directness in all responses, delivering complete information while matching response depth to the task's complexity.
For straightforward queries, keep answers brief - typically a few lines excluding code or tool invocations. Expand detail only when dealing with complex work or when explicitly requested.
Optimize for conciseness while preserving helpfulness and accuracy. Address only the immediate request, omitting unrelated details unless critical. Target 1-3 sentences for simple answers when possible.
Avoid extraneous framing - skip unnecessary introductions or conclusions unless requested. After completing file operations, confirm completion briefly rather than explaining what was done. Respond directly without phrases like "Here's the answer:", "The result is:", or "I will now...".
Example responses demonstrating appropriate brevity:
<communicationExamples>
User: `what's the square root of 144?`
Assistant: `12`
User: `which directory has the server code?`
Assistant: [searches workspace and finds backend/]
`backend/`

User: `how many bytes in a megabyte?`
Assistant: `1048576`

User: `what files are in src/utils/?`
Assistant: [lists directory and sees helpers.ts, validators.ts, constants.ts]
`helpers.ts, validators.ts, constants.ts`

</communicationExamples>

When executing non-trivial commands, explain their purpose and impact so users understand what's happening, particularly for system-modifying operations.
Do NOT use emojis unless explicitly requested by the user.

</communicationStyle>
<notebookInstructions>
To edit notebook files in the workspace, you can use the edit_notebook_file tool.
Use the run_notebook_cell tool instead of executing Jupyter related commands in the Terminal, such as `jupyter notebook`, `jupyter lab`, `install jupyter` or the like.
Use the copilot_getNotebookSummary tool to get the summary of the notebook (this includes the list or all cells along with the Cell Id, Cell type and Cell Language, execution details and mime types of the outputs, if any).
Important Reminder: Avoid referencing Notebook Cell Ids in user messages. Use cell number instead.
Important Reminder: Markdown cells cannot be executed
</notebookInstructions>
<outputFormatting>
Use proper Markdown formatting: - Wrap symbol names (classes, methods, variables) in backticks: `MyClass`, `handleClick()`
- When mentioning files or line numbers, always follow the rules in fileLinkification section below:<fileLinkification>
When mentioning files or line numbers, always convert them to markdown links using workspace-relative paths and 1-based line numbers.
NO BACKTICKS ANYWHERE:
- Never wrap file names, paths, or links in backticks.
- Never use inline-code formatting for any file reference.

REQUIRED FORMATS:
- File: [path/file.ts](path/file.ts)
- Line: [file.ts](file.ts#L10)
- Range: [file.ts](file.ts#L10-L12)

PATH RULES:
- Without line numbers: Display text must match the target path.
- With line numbers: Display text can be either the path or descriptive text.
- Use '/' only; strip drive letters and external folders.
- Do not use these URI schemes: file://, vscode://
- Encode spaces only in the target (My File.md → My%20File.md).
- Non-contiguous lines require separate links. NEVER use comma-separated line references like #L10-L12, L20.
- Valid formats: [file.ts](file.ts#L10) or [file.ts#L10] only. Invalid: ([file.ts#L10]) or [file.ts](file.ts)#L10

USAGE EXAMPLES:
- With path as display: The handler is in [src/handler.ts](src/handler.ts#L10).
- With descriptive text: The [widget initialization](src/widget.ts#L321) runs on startup.
- Bullet list: [Init widget](src/widget.ts#L321)
- File only: See [src/config.ts](src/config.ts) for settings.

FORBIDDEN (NEVER OUTPUT):
- Inline code: `file.ts`, `src/file.ts`, `L86`.
- Plain text file names: file.ts, chatService.ts.
- References without links when mentioning specific file locations.
- Specific line citations without links ("Line 86", "at line 86", "on line 25").
- Combining multiple line references in one link: [file.ts#L10-L12, L20](file.ts#L10-L12, L20)


</fileLinkification>
Use KaTeX for math equations in your answers.
Wrap inline math equations in $.
Wrap more complex blocks of math equations in $$.

</outputFormatting>


[copilot_cache_control: { type: 'ephemeral' }]
~~~

### User
~~~md
<environment_info>
The user's current OS is: macOS
</environment_info>
<workspace_info>
I am working in a workspace with the following folders:
- /Users/suhyun/minGPT_copy 
I am working in a workspace that has the following structure:
```
demo.ipynb
generate.ipynb
LICENSE
README.md
setup.py
mingpt/
	__init__.py
	bpe.py
	model.py
	trainer.py
	utils.py
projects/
	readme.md
	adder/
		adder.py
		readme.md
	chargpt/
		chargpt.py
		readme.md
tests/
	test_huggingface_import.py
```
This is the state of the context at this point in the conversation. The view of the workspace structure may be truncated. You can use tools to collect more context if needed.
</workspace_info>


[copilot_cache_control: { type: 'ephemeral' }]
~~~

### User
~~~md
<context>
The current date is January 29, 2026.
Terminals:
Terminal: zsh
Terminal: zsh

</context>
<editorContext>
The user's current file is /Users/suhyun/minGPT_copy/README.md. 
</editorContext>
<reminderInstructions>
When using the replace_string_in_file tool, include 3-5 lines of unchanged code before and after the string you want to replace, to make it unambiguous which part of the file should be edited.
For maximum efficiency, whenever you plan to perform multiple independent edit operations, invoke them simultaneously using multi_replace_string_in_file tool rather than sequentially. This will greatly improve user's cost and time efficiency leading to a better user experience. Do not announce which tool you're using (for example, avoid saying "I'll implement all the changes using multi_replace_string_in_file").
Do NOT create a new markdown file to document each change or summarize your work unless specifically requested by the user.

</reminderInstructions>
<userRequest>
Make a change in one of the recommended files, and commit with a meaningful message. Recommended files:
mingpt/model.py
mingpt/utils.py
train.py
README.md

</userRequest>


[copilot_cache_control: { type: 'ephemeral' }]
~~~



## Response
### Assistant
~~~md
I'll make a meaningful change to one of the recommended files and commit it. Let me first check what's in these files to understand the project better. 
🛠️ read_file (toolu_01USdtudYLddErXqSS6irWbX) {
  "filePath": "/Users/suhyun/minGPT_copy/README.md",
  "startLine": 1,
  "endLine": 50
}
🛠️ read_file (toolu_01Exg5j84xoUWf8mHNjd6qGj) {
  "filePath": "/Users/suhyun/minGPT_copy/mingpt/utils.py",
  "startLine": 1,
  "endLine": 50
}
🛠️ file_search (toolu_01KPZ4yiCcyLVcPyuxcnnzij) {
  "query": "train.py"
}
~~~


<style>
[id^="system"], [id^="user"], [id^="assistant"] {
		margin: 4px 0 4px 0;
}

.markdown-body > pre {
		padding: 4px 16px;
}
</style>
