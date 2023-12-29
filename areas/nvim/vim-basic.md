---
id: vim-basic
slug: vim-basic
createdDate: 2023-09-02
updatedDate: 2023-12-29
---

# Vim

> about vim features

## Vimrc

| lua                             | vimscript          | desc                                                   |
|---------------------------------|--------------------|--------------------------------------------------------|
| `vim.opt.path:append({ "**" })` | `set path+=**`     | vimgrep 으로 파일을 recursive하게 찾을 수 있게 해준다. |
|                                 | `set wildmenu`     | vim에서 자동완성된 결과들을 nvim처러 보여줌            |
|                                 | `set nocompatible` | 오리지날 vi와 호환하지 않음                            |
|                                 | `set nobackup`     | 백업파일을 만들지 않음                                 |


## Normal Mode

| key combination | desc                                                |
|-----------------|-----------------------------------------------------|
| vab             | selects content within parenthesis () along with it |
| vib             | selects only the content within parenthesis         |
| vaB             | for {}                                              |
| viB             | for {}                                              |
| gf              | jumps to the file when on the file path.            |
| gg=G or G=gg    | indent code in current buffer                       |


## Insert Mode

| key combination | desc                                        |
|-----------------|---------------------------------------------|
| ctrl + w        | delete previous word                        |
| ctrl + u        | delete from cursor to beginning of the line |
|                 |                                             |


## Command Mode

| key combination          | desc                                   |
|--------------------------|----------------------------------------|
| :h v_<Esc>               | help page for visual mode esc          |
| :h i_<Esc>               | help page for insert mode esc          |
| :h ctrl-w_c              | help page for close current window     |
| :verbose map ds          | verbose한 정보로 ds에 매핑된 키를 확인 |
| :help keycodes :help <C- | more information on the key codes      |


## Quick fix list

`:vimgrep /<regex>/ <filename>`
- `:vimgrep`대신에 `:vim`으로 사용가능

```bash
:vimgrep /test/g src/** # src 밑에서 검색
:vimgrep /test/g **/*  # 전체에서 검색
:vimgrep /test/g % # 현재 열린 버퍼내에서 검색

:Telescope quickfix # Telescope에서 검색
:copen # quick fix list 열기
```


## inlay hint
0.10 version 이후부터 사용가능(2023/10/03 기준으로는 nightly version)
```lua
-- :help inlay_hint
if vim.lsp.inlay_hint then
  vim.keymap.set(
    "n",
    "<leader>uh",
    function() vim.lsp.inlay_hint(0, nil) end, -- 0은 현재 buffer 지정
    { desc = "Toggle Inlay Hints" }
  )
end
```

```lua
-- https://github.com/typescript-language-server/typescript-language-server#inlay-hints-textdocumentinlayhint
tsserver = {
  typescript = {
    inlayHints = {
      includeInlayEnumMemberValueHints: true;
      includeInlayFunctionLikeReturnTypeHints: true;
      includeInlayFunctionParameterTypeHints: true;
      includeInlayParameterNameHints: 'all',
      includeInlayParameterNameHintsWhenArgumentMatchesName: true;
      includeInlayPropertyDeclarationTypeHints: true;
      includeInlayVariableTypeHints: true;
      includeInlayVariableTypeHintsWhenTypeMatchesName: true;
    },
    javascript = {
      inlayHints = {
        includeInlayEnumMemberValueHints: true;
        includeInlayFunctionLikeReturnTypeHints: true;
        includeInlayFunctionParameterTypeHints: true;
        includeInlayParameterNameHints: 'all',
        includeInlayParameterNameHintsWhenArgumentMatchesName: true;
        includeInlayPropertyDeclarationTypeHints: true;
        includeInlayVariableTypeHints: true;
        includeInlayVariableTypeHintsWhenTypeMatchesName: true;
      },
    }
  }
}
```

## fold

### fold setup
```lua
vim.cmd([[
  set foldmethod=expr
  set foldexpr=nvim_treesitter#foldexpr()
  set nofoldenable
  set foldcolumn=1
]])

-- Auto Commands for Fold
vim.api.nvim_create_autocmd({ "BufWinLeave" }, {
  pattern = { "*.*" },
  desc = "save view (folds), when closing file",
  command = "mkview",
})
vim.api.nvim_create_autocmd({ "BufWinEnter" }, {
  pattern = { "*.*" },
  desc = "load view (folds), when opening file",
  command = "silent! loadview",
})
```
