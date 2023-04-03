---
layout: post
ghissueid: 8
title: Installing LunarVim with Github Copilot on Ubuntu 22.04 via Putty 
---

I spent several hours setting up this combo on my home server, it's finally working.

My home server is running Ubuntu 22.04, I access it remotely with Putty.  I can technically hook up a keyboard/mouse/monitor and install a window manager then just set everything up the desktop way but I was too lazy to do that.


## NeoVim

Do not use `sudo apt install`, it will install an older version of NeoVim.  Instead download appimage from github, then unpack it and copy it to /usr/local.

    # download nvim.appimage from github
    ./nvim.appimage --appimage-extract
    sudo cp -rp squashfs-root/usr/* /usr/local/

Test with nvim to confirm it is installed.

## LunarVim

Just copy the installation command from [https://www.lunarvim.org/docs/installation](https://www.lunarvim.org/docs/installation) and run it.  Answer yes when prompted.

Launch lvim to verify.

## Github Copilot

While in lvim dashboard (default with no file), press 'c' for configuration file.  This will edit `~/.config/lvim/config.lua`, add:

```
lvim.plugins = {
    {
      "github/copilot.vim",
    },
}
vim.g.copilot_no_tab_map = true
vim.g.copilot_assume_mapped = true
vim.g.copilot_tab_fallback = ""
local cmp = require "cmp"
lvim.builtin.cmp.mapping["<Tab>"] = function(fallback)
  if cmp.visible() then
    cmp.select_next_item()
  else
    local copilot_keys = vim.fn["copilot#Accept"]()
    if copilot_keys ~= "" then
      vim.api.nvim_feedkeys(copilot_keys, "i", true)
    else
      fallback()
    end
  end
end
```

to (near the end of) the file.

Then do a `:Copilot setup`, this will be un-successful because it will try to connect to Github via browser.  But I was connected to the server with ssh and don't have any browser on my server.  See Next step.

## Browser

To run a GUI I need to run an X server.  I use XMing on Windows. I have to enable X11 forwarding (in Connection-SSH-X11) in Putty.  I run xclock to verify it works.  Now all I have to do is install a browser on the server.

It should be simple right? Just `sudo apt install firefox`.  However the browser installed this way will not work. For some reason it can not connect to X server locally.

When using `apt install`, both firefox and Chromium will be installed from Snap, I got so confused and mad I completely deleted snap from my server.

I installed firefox with mozilla ppa (just google non-snap firefox), it successfully run on my windows, but it's so slow it's unusable.

I ended up donwload chrome from [google](https://www.google.com/chrome/?platform=linux) (actualy get the link by copy link from downloaded file in Windows Chrome then wget the link).  It finally worked fine.

Inside lvim, do `:Copilot seteup` again, everthing was set up.
