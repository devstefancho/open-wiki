---
id: nvim-dap
slug: nvim-dap
createdDate: 2023-09-02
updatedDate: 2023-12-29
---

## Install

You must install `mxsdev/nvim-dap-vscode-js` and `microsoft/vscode-js-debug` for working in javascript (these will offer default configuration and adapters)

### Packer
```lua
  -- Debugger
  use("mfussenegger/nvim-dap") -- core
  use("rcarriga/nvim-dap-ui") -- debugger ui
  use("folke/neodev.nvim") -- this is only for nvim-dap-ui
  use("nvim-telescope/telescope-dap.nvim") -- command show in telescope
  use("theHamsta/nvim-dap-virtual-text") -- works like REPL(Read-eval-print loop)
  use("mxsdev/nvim-dap-vscode-js") -- need to debug javascript
  use({
    "microsoft/vscode-js-debug",
    opt = true,
    run = "npm install --legacy-peer-deps && npm run compile",
  }) -- this is only for mxsdev/nvim-dap-vscode-js
```

## Configurations

### Setup plugins
```lua
local status, dap = pcall(require, "dap")
if not status then
  return
end

require("neodev").setup({
  library = { plugins = { "nvim-dap-ui" }, types = true },
  ...,
})
require("dapui").setup()
require("nvim-dap-virtual-text").setup()
require("telescope").load_extension("dap")

-- See https://github.com/mxsdev/nvim-dap-vscode-js#installation
require("dap-vscode-js").setup({
  adapters = { "pwa-node", "pwa-chrome", "pwa-msedge", "node-terminal", "pwa-extensionHost" }, -- which adapters to register in nvim-dap
})
```

### Add javascript configuration
```lua
for _, language in ipairs({ "typescript", "javascript" }) do
  require('dap').configurations[language] = {
    {
      type = "pwa-node",
      request = "launch",
      name = "Launch file",
      program = "${file}",
      cwd = "${workspaceFolder}",
    },
    {
      type = "pwa-node",
      request = "attach",
      name = "Attach",
      processId = require("dap.utils").pick_process,
      cwd = "${workspaceFolder}",
    },
  }
end
```

### dap-ui open and close
"dapui_config" key name is not essential (this means you can change it to any name), but you recommend to use this name. If you want to more about the reason then see [this tj devries video starting from 52:00](https://youtu.be/0moS8UHupGc?t=3129)
```lua
-- See :help dap-extensions
dap.listeners.after.event_initialized["dapui_config"] = function()
  dap_ui.open() -- this will start when nvim-dap start
end

dap.listeners.before.event_terminated["dapui_config"] = function()
  dap_ui.close()
end
```

### Keymaps
global keymap is not recommend but this time set keymaps for globally, because you may want to set break point in any type of files
(so don't add a option as `{ buffer = 0}`)
```lua
-- [[ Keymaps ]]
local keymap = function(lhs, rhs, desc)
  if desc then
    desc = "[DAP] " .. desc
  end

  vim.keymap.set("n", lhs, rhs, { silent = true, desc = desc })
end

keymap("<leader><leader>db", dap.step_back, "step_back")
keymap("<leader><leader>di", dap.step_into, "step_into")
keymap("<leader><leader>do", dap.step_over, "step_over")
keymap("<leader><leader>du", dap.step_out, "step_out")
keymap("<leader><leader>dc", dap.continue, "continue")
keymap("<leader><leader>dr", dap.repl.open)
keymap("<leader><leader>dt", dap.toggle_breakpoint)
```

### Setup Nerd-Font for nvim-dap-ui
You should download nerd-font for icons
I install "JetBrains Mono", it perfectly work for nvim-dap-ui
See [[iterm2-nerd-fonts]]

## How to use
1. add break point (toggle_breakpoint)
2. open dap-ui (this will opened automatically by listeners)
3. select configuration name from telescope
4. start debugger (dap.continue)


## Trouble Shootingnvim dap is debbuger for neovim

### No configuration found for python you need to add configs to dap.configurations.javascript
if you don't have configuration and adapters this error will appear
so you need to install `mxsdev/nvim-dap-vscode-js` and `microsoft/vscode-js-debug`

### Launch file 했을때, entrypoint error 발생하는 경우

#### Error
`[dap-js] Error trying to launch JS debugger: ...nvim/lazy/nvim-dap-vscode-js/lua/dap-vscode-js/utils.lua:64: Debugger entrypoint file '/Users/stefancho/.local/share/nvim/lazy/vscode-js-debug/out/src/vsDebugServer.js' does not exist. Did it build properly`

#### debugger_path 지정하기
packer로 설치하지 않은 경우에는 [Manual install](https://github.com/mxsdev/nvim-dap-vscode-js#manually)을 해줘야하기 때문에
직접 debugger_path를 지정해줘야한다.
```lua
require("dap-vscode-js").setup({
  debugger_path = "/Users/stefancho/.local/share/nvim/lazy/vscode-js-debug", -- lazy.nvim으로 설치했기 때문에 여기에 vscode-js-debug가 설치됨
  adapters = { "pwa-node", "pwa-chrome", "pwa-msedge", "node-terminal", "pwa-extensionHost" }, -- which adapters to register in nvim-dap
})
```

#### build 하기
```bash
cd /Users/stefancho/.local/share/nvim/lazy/vscode-js-debug
npm install --legacy-peer-deps
npx gulp vsDebugServerBundle
mv dist out
```

## Ref
- https://github.com/mfussenegger/nvim-dap/wiki/Debug-Adapter-installation#javascript
- https://github.com/mxsdev/nvim-dap-vscode-js#installation
