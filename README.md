# MCP_server_knowhow
Goal is to practice different implementations using the MCP server

**How to create an MCP server and connect to an LLM of your choice?**

1. We initialize a project for creating an MCP server, and we preferably do it using uv.
  
2. uv init name-mcp-server (whatever name we choose goes here instead of <name-mcp-server>)
   We get an outcome: Initialized project 'name-mcp-server' at 'location', i.e., path where we saved our MCP server.

3. It initializes the server with these files by default: main.py, pyproject.toml, and README.md.

4. cd to the directory where our MCP server resides.
   
5. If we would like to test our code using the MCP Inspector, we could do this: uv add "mcp[cli]".

6. Create a virtual environment in a preferred programming language, say, Python.

7. Activate the virtual environment. If the python version requested in the pyproject.toml file in the MCP server directory does not match the python version in the virtual environment, we will get an error message as follows:
    
    Because the requested Python version (>=3.8.19) does not satisfy Python>=3.10 and your project depends on mcp[cli], we can conclude
    that your project's requirements are unsatisfiable.
   
8. We can fix the issue by changing the version of python in the .toml file. If we still see the issue popping up, we can try pinning down the version in the virtual python environment like this: uv python pin 3.12

9. Deactivate the virtual python environment and try activating it again to see if the error still persists.

10. touch some_mcp_file.py (where we save the resources, tools, and other functionalities we plan to implement through our MCP server).

11. Navigate to the some_mcp_file.py and create those functionalities.

12. Adding the following in the some_mcp_file.py is important as this code block acts as the "Launch Button" for our server. It tells python exactly when to start the server and how to talk to the LLM of our choice.

    if __name__ == "__main__":
    
        mcp.run(transport="stdio")

14. If we would like to test the code using the MCP Inspector, we run the following command:

    uv run --with "mcp[cli]" mcp dev some_mcp_file.py

15. In parallel, we need to create a config file for our LLM to access. Let's see a sample code for Claude:

    mkdir -p ~/Library/Application\ Support/Claude 
    touch ~/Library/Application\ Support/Claude/claude_desktop_config.json 

    FYI: This config file tells Claude exactly three things:
    A. The Name: "I have a tool called name-mcp-server."
    B. The Command: "To use it, you need to run /path/to/python."
    C. The Arguments: "And you must run this specific script: some_mcp_file.py."

17. Can access the config file using: open -e ~/Library/Application\ Support/Claude/claude_desktop_config.json to create the following:
    { 
    "mcpServers": 
    { 
    "name-mcp-server": #the name of the MCP server created
    { 
    "command": "/path/name-mcp-server/.venv/bin/python", #provide the entire path to the python virtual environment; <path> is a placeholder
    "args": ["/path/name-mcp-server/some_mcp_file.py"] #provide the entire path to the python file saved in the MCP server directory; <path> is a placeholder
    } 
    } 
    }

18. When all this is done, we finally run the following:

    uv run some_mcp_file.py
This code here ensures the tools exist, launches the code in a bubble, and opens the "phone line" for Claude to talk to it.


