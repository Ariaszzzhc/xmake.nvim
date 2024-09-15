<p align="right">
    <b>| English | <a href="README_zh.md">简体中文</a> |</b>
</p>

<h1 align="center">Xmake.nvim</h1>

> [!Note]
> Now that the main branch of the plugin is being refactored, to use the plugin properly, you can use the `branch` and `version` functions of the plugin manager.
>
> `baranch = "v2"` or `version = "^2"`.

## 🎐 Features

1. Provide UI interface for you to quickly configure, compile and clean up xmake.
2. automatically generates `compile_commands.json` for _lsp_ use when saving `xmake.lua` files
3. All external command calls are executed asynchronously without worrying about performance issues.

<table>
    <tr>
        <th>Set Menu</th>
        <th>Set Toolchain</th>
    </tr>
    <tr>
        <td>
            <img src="./assets/XmakeSetMenu.png" />
        </td>
        <td>
            <img src="./assets/XmakeSetToolchain.png" />
        </td>
    </tr>
    <tr>
        <th>Set Build Mode</th>
        <th>Build Target</th>
    </tr>
    <tr>
        <td>
            <img src="./assets/XmakeSetMode.png" />
        </td>
        <td>
            <img src="./assets/XmakeBuildTarget.png" />
        </td>
    </tr>
</table>

<details><summary>Gif Preview</summary></details>

![XmakePreviewGif](./assets/XmakePreview.gif)

</details>

# 🏗 Installation

### [💤lazy.nvim](https://github.com/folke/lazy.nvim):

```lua
{
    "Mythos-404/xmake.nvim",
    lazy = true,
    event = "BufReadPost xmake.lua",
    config = true,
    dependencies = { "MunifTanjim/nui.nvim", "nvim-lua/plenary.nvim" },
}
```

> The plugin uses the new command execution function `vim.system` so your version of _neovim_ must be built after this [commit](https://github.com/neovim/neovim/pull/23827)!
> If this function is not supported you can use [xmake.nvim](https://github.com/Mythos-404/xmake.nvim/tree/v1) from the v1 branch.

## ⚙️ Default settings

```lua
{
    files_path = vim.fn.stdpath("cache") .. "/xmake_", -- project data saved by plugin

    compile_command = { -- compile_command file generation configuration
        lsp = "clangd", -- generate compile_commands file for which lsp to read
        dir = ".vscode", -- location of the generated file
    },

    menu = { -- interface configuration
        size = { width = 25, height = 20 }, -- interface size
        bottom_text_format = "%s(%s)", -- interface formatting string Generated by default: `"xmake_test(debug)"`
        border_style = { "╭", "─", "╮", "│", "╯", "─", "╰", "│" }, -- interface border see nui.nvim documentation for more detail
    },

    debug = false, -- Enable to provide more detailed error output.

    work_dir = vim.fn.getcwd(), -- Get the work directory.
}
```

## 💡 Commands

1. `XmakeSetMenu` General Selection Page
2. `XmakeSetToolchain` toolchain selection
3. `XmakeSetMode` compile mode selection
4. `XmakeSetTarget` target selection
5. `XmakeSetPlat` Target platform selection
6. `XmakeSetArch` Target architecture selection
7. `XmakeBuild` Build target
8. `XmakeBuildAll` Build all targets.
9. `XmakeBuildTarget` Builds a specific target.
10. `XmakeClean` Cleans up a target.
11. `XmakeCleanAll` Cleans all targets.
12. `XmakeCleanTarget` Cleans the specified target.

## ✨ Use with other plugins

Use with `nvim-dap` to get the compiled output path of a target.

```lua
dap.configurations.cpp = {
    {
        name = "Launch file",
        type = "codelldb",
        request = "launch",
        program = function()
            return require("xmake.project").info.target.exec_path
        end,
        cwd = "${workspaceFolder}",
        stopOnEntry = false,
    },
}
```

Used in conjunction with state line plugins such as `lualine.nvim`, an example of `lualine.nvim` is provided here

```lua
local xmake_component = {
    function()
        local xmake = require("xmake.project").info
        if xmake.target.tg == "" then
            return ""
        end
        return xmake.target.tg .. "(" .. xmake.mode .. ")"
    end,

    cond = function()
        return vim.o.columns > 100
    end,

    on_click = function()
        require("xmake.project._menu").init() -- Add the on-click ui
    end,
}

require("lualine").setup({
    sections = {
        lualine_y = {
            xmake_component,
        },
    },
})
```

## Todo

-   [ ] Run function (in UI)
    -   [ ] Run the target
    -   [ ] Run multiple targets
    -   [ ] Input when running
    -   [ ] Monitor the success of the run

## 🎉 Other similar projects

-   [CnsMaple/xmake.nvim](https://github.com/CnsMaple/xmake.nvim)
