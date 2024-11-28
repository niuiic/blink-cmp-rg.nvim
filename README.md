# blink-cmp-rg.nvim

Ripgrep source for [blink.cmp](https://github.com/Saghen/blink.cmp).

```lua
require("blink.cmp").setup({
	sources = {
		completion = {
			enabled_providers = { "lsp", "path", "snippets", "buffer", "ripgrep" }, -- add "ripgrep" here
		},
		providers = {
			-- other sources
			ripgrep = {
				module = "blink-cmp-rg",
				name = "Ripgrep",
				-- options below are optional, these are the default values
				---@type blink-cmp-rg.Options
				opts = {
					-- blink.cmp get prefix in a different way,
					-- thus use `prefix_min_len` instead of `min_keyword_length`
					prefix_min_len = 3,
					get_command = function(context, prefix)
						return {
							"rg",
							"--no-config",
							"--json",
							"--word-regexp",
							"--ignore-case",
							"--",
							prefix .. "[\\w_-]+",
							vim.fs.root(0, ".git") or vim.fn.getcwd(),
						}
					end,
					get_prefix = function(context)
						return context.line:sub(1, context.cursor[2]):match("[%w_-]+$") or ""
					end,
				},
			},
		},
	},
})
```
