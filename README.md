## Setup Compilers

**MSVC:**

1. Download the [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
2. Select the ``Desktop development with C++`` workload
3. Select the ``MSVC Build Tools`` and the ``Windows SDK``

**Clang:**
1. Go to the [llvm downloads](https://github.com/llvm/llvm-project/releases) on github
2. Download the ``LLVM-X.X.X-win64.exe`` windows installer file
3. Run the installer and choose ``Add LLVM to the system PATH for all users`` when prompted

## Setup Build Systems

**XMake:**

1. Run ``winget install xmake`` in the terminal
2. Check installation with ``xmake --version`` in the terminal

## Setup Visual Studio Code

**XMake:**

1. Install the [Microsoft C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) for vscode
2. Install the [XMake Extension](https://marketplace.visualstudio.com/items?itemName=tboox.xmake-vscode) for vscode
3. Generate ``.vscode/compile_commands.json`` by running ``xmake project -k compile_commands .vscode``
4. For automatic updates add this line to your xmake file ``add_rules("plugin.compile_commands.autoupdate", { outputdir = ".vscode" })``
5. Now add the ``.vscode/compile_commands.json`` to your ``.gitignore`` file
6. Create a file called ``.vscode/c_cpp_properties.json`` with this json:
```json
{
    "version": 4,
    "configurations":
    [
        {
            "name": "xmake",
            "compileCommands": ".vscode/compile_commands.json"
        }
    ]
}
```
