# Debugging Lua Neovim Configuration

In order to debug Neovim configuration we need to setup a Lua debug server and
a client to connect to it. This guide will show you how to do that.

## Setup Breakpoints for Configuration

Once we have the configuration file we want to debug, we need to setup a debug
breakpoint.

## Start Debug Server

To start the debug server, we need to run another instance of Neovim that will
trigger the breakpoint. In our case, we were debugging some
[nvim-dev-container](https://github.com/esensar/nvim-dev-container) code, so
we just need to open a project that will trigger the breakpoint on the function
`add_neovim` when executing the command `DevContainerAttach` in the command
line.

Once we have found the project that will trigger the breakpoint, we need to
start the debug server. To do that, we need to run the following command to
start the lua debug server (I'm using [one small step for vimkind](
https://github.com/jbyuki/one-small-step-for-vimkind ) ), or, with my current
keybindings, pressing `<leader>bsl` (breakpoint start lua(server)):

```lua
require('osv').launch { port = 8086 }
```

## Start Debug Client

Now, on the other instance of Neovim were we have set the breakpoint, we need
to attach to the debug server. Which if we have correctly setup the DAP
adapter, for example:

```lua
dap.adapters.nlua = function(callback, config)
  callback { type = 'server', host = config.host or '127.0.0.1', port = config.port or 8086 }
end

dap.configurations['lua'] = {
  {
    type = 'nlua',
    request = 'attach',
    name = 'Attach to running Neovim instance',
  },
}
```

The debugger should start, and after triggering the command in the server side,
we should see the debugger stopping in our client.
