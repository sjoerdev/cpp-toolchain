**MSVC Compiler:**

1. Download the [Microsoft C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
2. Select the ``Desktop development with C++`` workload
3. Select the ``MSVC Build Tools`` and the ``Windows SDK``

**XMake Build System:**

1. Run ``winget install xmake`` in the terminal
2. Check installation with ``xmake --version`` in the terminal

**(Optional) Setup OpenGL Project:**
> you can skip the following steps by using [this](https://github.com/sjoerdev/opengl-template-xmake) template project
1. Create a project directory with xmake by running ``xmake create project`` in the terminal
2. Move inside the directory by running ``cd project`` in the terminal
3. Setup the ``xmake.lua`` file like so:
```lua
-- dependencies
add_requires("glm")
add_requires("glfw")
add_requires("glad", { configs = { api = "gl=3.3", profile = "core", spec = "gl"} })
add_requires("imgui", { configs = { opengl3 = true, glfw = true } })

-- project
target("project")

    -- settings
    set_kind("binary")
    set_languages("c++17")

    -- sources
    add_files("src/*.cpp")

    -- linking
    add_packages("glfw", "glad", "imgui", "glm")
```
4. Now create an opengl hello world like this:
```cpp
#include <iostream>
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include <imgui.h>
#include <imgui_impl_glfw.h>
#include <imgui_impl_opengl3.h>
#include <glm/vec3.hpp>
#include <glm/vec4.hpp>
#include <glm/mat4x4.hpp>
#include <glm/ext/matrix_transform.hpp>
#include <glm/ext/matrix_clip_space.hpp>
#include <glm/ext/scalar_constants.hpp>

// resizing callback
void resize(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}

int main()
{
    // try init glfw
    if (!glfwInit())
    {
        printf("failed to init glfw");
        return -1;
    }

    // set opengl version
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    // create window
    GLFWwindow* window = glfwCreateWindow(800, 600, "program", nullptr, nullptr);
    if (!window)
    {
        printf("failed to create glfw window");
        glfwTerminate();
        return -1;
    }

    // set opengl context
    glfwMakeContextCurrent(window);

    // load opengl with glad
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        printf("failed to init glad");
        return -1;
    }

    // framebuffer size callback
    glfwSetFramebufferSizeCallback(window, resize);

    // init imgui
    IMGUI_CHECKVERSION();
    ImGui::CreateContext();
    ImGuiIO& io = ImGui::GetIO();
    ImGui_ImplGlfw_InitForOpenGL(window, true);
    ImGui_ImplOpenGL3_Init("#version 330"); // GLSL version

    // render loop
    while (!glfwWindowShouldClose(window))
    {
        // poll events
        glfwPollEvents();

        // clear screen
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // imgui
        ImGui_ImplOpenGL3_NewFrame();
        ImGui_ImplGlfw_NewFrame();
        ImGui::NewFrame();
        ImGui::ShowDemoWindow();
        ImGui::Render();
        ImGui_ImplOpenGL3_RenderDrawData(ImGui::GetDrawData());

        // swap buffers
        glfwSwapBuffers(window);
    }

    // cleanup
    ImGui_ImplOpenGL3_Shutdown();
    ImGui_ImplGlfw_Shutdown();
    ImGui::DestroyContext();
    glfwDestroyWindow(window);
    glfwTerminate();
    return 0;
}
```
5. Now run ``xmake`` to build the project
6. And then run ``xmake run`` to run the project

**(Optional) Setup Visual Studio Code:**

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
