# Vim

- [Practice](https://openvim.com/sandbox.html)
- [Vim Cheat Sheet](https://vim.rtorr.com/)
- [Text Selection](https://www.cs.swarthmore.edu/oldhelp/vim/selection.html)

## [NeoVim](https://neovim.io/)

```sh
sudo snap install nvim --classic
```

```sh
# customization
~/.config/nvim/init.lua
```

### [ThePrimeagen's init.lua](https://github.com/ThePrimeagen/init.lua)

[0 to LSP : Neovim RC From Scratch](https://www.youtube.com/watch?v=w7i4amO_zaE&t=960s)

[init.lua](https://github.com/ThePrimeagen/init.lua)

### [Packer - Package manager](https://github.com/wbthomason/packer.nvim)

```sh
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```

```md
: PakerSync
: Mason # install language server
```

### [Telescope](https://github.com/nvim-telescope/telescope.nvim)

```lua
#  ~/.config/nvim/after/plugin/telescope.lua

local builtin = require('telescope.builtin')
vim.keymap.set('n', '<leader>pf', builtin.find_files, {})
vim.keymap.set('n', '<C-p>', builtin.git_files, {})
vim.keymap.set('n', '<leader>ps', function()
	 builtin.grep_string({ search = vim.fn.input(" Gerp > ") } );
end)
```

```sh
: Telescope keymaps
: Talescope search_tags
: Telescope builtin
: help <text>
```


### [catppuccin](https://github.com/catppuccin/nvim)

```lua
-- ~/.config/nvim/lua/otool/packer.lua
use { "catppuccin/nvim", as = "catppuccin" }
-- :so
-- :PackerSync
```

```lua
-- ~/.config/nvim/after/plugin/colors.lua
require('catppuccin').setup({
    disable_background = true
})

function ColorMyPencils(color) 
	color = color or "catppuccin"
	vim.cmd.colorscheme(color)

	vim.api.nvim_set_hl(0, "Normal", { bg = "none" })
	vim.api.nvim_set_hl(0, "NormalFloat", { bg = "none" })

end

ColorMyPencils()

-- so:
```

### [treesitter](https://github.com/nvim-treesitter/nvim-treesitter/wiki/Installation)


```lua
-- ~/.config/nvim/lua/otool/packer.lua
    use {
        'nvim-treesitter/nvim-treesitter',
        run = function()
            local ts_update = require('nvim-treesitter.install').update({ with_sync = true })
            ts_update()
        end,
    }
-- :so
-- :PackerSync
```

```lua
-- :lua ColorMyPencils()
```

```lua
-- ~/.config/nvim/after/plugin/treesitter.lua
require'nvim-treesitter.configs'.setup {
  -- A list of parser names, or "all"
  ensure_installed = { "vimdoc", "python", "bash", "java", "sql", "lua", "rust" },

  -- Install parsers synchronously (only applied to `ensure_installed`)
  sync_install = false,

  -- Automatically install missing parsers when entering buffer
  -- Recommendation: set to false if you don't have `tree-sitter` CLI installed locally
  auto_install = true,

  highlight = {
    -- `false` will disable the whole extension
    enable = true,

    -- Setting this to true will run `:h syntax` and tree-sitter at the same time.
    -- Set this to `true` if you depend on 'syntax' being enabled (like for indentation).
    -- Using this option may slow down your editor, and you may see some duplicate highlights.
    -- Instead of true it can also be a list of languages
    additional_vim_regex_highlighting = false,
  },
}
-- :so
```

### [treesitter - playground](https://github.com/nvim-treesitter/playground)

```lua
-- ~/.config/nvim/lua/otool/packer.lua
use("nvim-treesitter/playground")
-- :so
-- :PackerSync
-- :TSPlaygroundToggle
```

### [undotree](https://github.com/mbbill/undotree)

```lua
-- ~/.config/nvim/after/plugin/treesitter.lua
use("mbbill/undotree")
-- :so
```

```lua
-- ~/.config/nvim/after/plugin/after/plugin/undotree.lua
vim.keymap.set("n", "<leader>u", vim.cmd.UndotreeToggle)
```

### [lsp-zero](https://github.com/VonHeikemen/lsp-zero.nvim)

- [mason - install and manage LSP servers](https://github.com/williamboman/mason.nvim)

```lua
-- ~/.config/nvim/lua/otool/packer.lua
-- https://github.com/VonHeikemen/lsp-zero.nvim/blob/v3.x/doc/md/configuration-templates.md#primes-config
use {
  'VonHeikemen/lsp-zero.nvim',
  branch = 'v3.x',
  requires = {
    --- Uncomment these if you want to manage the language servers from neovim
    {'williamboman/mason.nvim'},
    {'williamboman/mason-lspconfig.nvim'},

    -- LSP Support
    {'neovim/nvim-lspconfig'},
    -- Autocompletion
    {'hrsh7th/nvim-cmp'},
    {'hrsh7th/cmp-nvim-lsp'},
    {'L3MON4D3/LuaSnip'},
  }
}
-- :so
-- :PackerSync
-- :Mason
```

```lua
-- ~/.config/nvim/after/plugin/lsp.lua
local lsp_zero = require('lsp-zero')

lsp_zero.on_attach(function(client, bufnr)
  local opts = {buffer = bufnr, remap = false}

  vim.keymap.set("n", "gd", function() vim.lsp.buf.definition() end, opts)
  vim.keymap.set("n", "K", function() vim.lsp.buf.hover() end, opts)
  vim.keymap.set("n", "<leader>vws", function() vim.lsp.buf.workspace_symbol() end, opts)
  vim.keymap.set("n", "<leader>vd", function() vim.diagnostic.open_float() end, opts)
  vim.keymap.set("n", "[d", function() vim.diagnostic.goto_next() end, opts)
  vim.keymap.set("n", "]d", function() vim.diagnostic.goto_prev() end, opts)
  vim.keymap.set("n", "<leader>vca", function() vim.lsp.buf.code_action() end, opts)
  vim.keymap.set("n", "<leader>vrr", function() vim.lsp.buf.references() end, opts)
  vim.keymap.set("n", "<leader>vrn", function() vim.lsp.buf.rename() end, opts)
  vim.keymap.set("i", "<C-h>", function() vim.lsp.buf.signature_help() end, opts)
end)

require('mason').setup({})
require('mason-lspconfig').setup({
  ensure_installed = {'bashls', 'pyright', 'rust_analyzer'},
  handlers = {
    lsp_zero.default_setup,
    lua_ls = function()
      local lua_opts = lsp_zero.nvim_lua_ls()
      require('lspconfig').lua_ls.setup(lua_opts)
    end,
  }
})

local cmp = require('cmp')
local cmp_select = {behavior = cmp.SelectBehavior.Select}

cmp.setup({
  sources = {
    {name = 'path'},
    {name = 'nvim_lsp'},
    {name = 'nvim_lua'},
    {name = 'luasnip', keyword_length = 2},
    {name = 'buffer', keyword_length = 3},
  },
  formatting = lsp_zero.cmp_format(),
  mapping = cmp.mapping.preset.insert({
    ['<C-p>'] = cmp.mapping.select_prev_item(cmp_select),
    ['<C-n>'] = cmp.mapping.select_next_item(cmp_select),
    ['<C-y>'] = cmp.mapping.confirm({ select = true }),
    ['<C-Space>'] = cmp.mapping.complete(),
  }),
})
-- :so
-- gd # goto definition
-- gr # go for references
```
