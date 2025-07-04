# Fix Gray-Level and Mura on Steam Deck

https://github.com/user-attachments/assets/254a30c3-f0c4-4e98-b25e-6407b6a3cd0f

### THIS METHOD IS DEPRECATED
Use a decky loader plugin instead
https://github.com/Moonveil-Kanata/MuraDeck

![vkbasalt vs gamescope steamos 3 7 8](https://github.com/user-attachments/assets/579503ce-43a8-42f9-93a2-52b0e4ee8e88)


## TL;DR

Average Samsung-panel on Steam Deck have known-issues called Mura Effect and raised gamma, where it has bad gray-uniformity on certain Hz and brightness. Resulting, grainy/dirty looks on the screen which can consider looks like a noise or film grain. Causing gradient effects on near-black looks like banding when fading into black. **While the current fix provided from valve unfortunately is raising the black level with the mura map keep showing on the black pixel.**

In a nutshell, **you can fix it by set your screen hertz between 47hz-66hz and set your brightness between 35/40%, and disable mura compensation on developer settings**. And you will get the screen where it should be, perfect black and nice gradient on near-black. **But this is not apply with higher brightness.**

This fix will use combination between **film grain for dithering, lift & gamma (only on near-black pixel)** and your **mura map (only on your bright pixel)**. While **this is not perfect**, atleast it will fix half of the screen issue.

Join the conversation here on [Reddit](https://www.reddit.com/r/SteamDeck/comments/1kb9gku/finally_able_to_fix_mura_effect_on_my_steam_deck/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) & [Reddit](https://www.reddit.com/r/SteamDeck/comments/1kfxuou/dithering_gamma_fix_mura_correction_for_steam/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) post.

<br>

# Usage
![vkbasalt vs gamescope](https://github.com/user-attachments/assets/0f9f2b69-042a-493e-bfe7-f2f9e791dd46)

We will use vkBasalt for better performance, you can use reshadeck/gamescope for easier usage, but it might causing stutter, performance penalty, etc.. especially on AAA games.

Reshade on vkBasalt works on vulkan layer (In-game only), while reshade on gamescope work as compositor layer (Whole screen) but it will causing overhead.

**Installation:**
1. Install [vkBasalt](https://github.com/simons-public/steam-deck-vkbasalt-install) locally
2. [Download Shaders](https://github.com/Moonveil-Kanata/muragraylevelfix-deck/releases/tag/shader), extract, and execute ``Install.sh``
2. Boot to game mode | Press ``STEAM`` Button → Settings → Developer → Activate ``Disable Mura Compensation``
4. **Done**, it should run on most of Steam games, you can toggle shader by pressing ``Tab``, read [troubleshoot](https://github.com/Moonveil-Kanata/muragraylevelfix-deck?tab=readme-ov-file#troubleshoot) for advanced stuff

**Optional:**
1. Press ``•••`` Disable frame limit and set to 60hz | **For less mura as possible**

![Mura Correction](https://github.com/user-attachments/assets/e47c2306-815d-40a8-a02c-71a47f05a66e)



<br>

# Troubleshoot
If you only play non HDR & Vulkan/DX Steam games, then the vkBasalt tutorial stop at the [Usage](https://github.com/Moonveil-Kanata/muragraylevelfix-deck?tab=readme-ov-file#usage) section.

Troubleshoot section is only, if you want all games work with vkbasalt, including OpenGL, Flatpak, Emu Deck, fix mura on HDR games etc. Or if something impossible with vkBasalt, we can use gamescope but with worse performance.

## Emu Deck
For Emu Deck after a couple test logging, it seems .appimage apps keep reading the x86 lib while they're running on x64. This method will make all emudeck emulators work with vkbasalt.

1. Navigate ``/home/deck/.local/share/vulkan/implicit_layer.d/``
2. rename file ``vkBasalt.x86.json`` to ``vkBasalt.x86.json.bak``

## HDR Games
HDR game have pale base rgb and higher mura-effect. So we will use different shader.
Set this as launch-command per-game with active compatible HDR games.
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_HDR.conf %command%
```

## Auto HDR Games
For some of you using [auto hdr reshade](https://www.reddit.com/r/SteamDeck/comments/1b4rbd8/auto_hdr_works_on_steam_deck_now/) (non vkBasalt)

Set this as launch-command per-game to use AutoHDR config
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_AutoHDR.conf %command%
```

## Gamescope
As this is using gamescope compositor itself, then it works on everything that shows on the screen. OpenGL, Nested Desktop, you name it, when launching the game.

**The downside of this method, it's tank your performance by a lot**

Usually I'll use this on app like Chrome, Nested Desktop, etc, while opengl games usually is an older game, so performance penaly shouldn't really visible

Set this as launch-command per-game
```
bash -c "DISPLAY=:0 xprop -root -f GAMESCOPE_RESHADE_EFFECT 8s -set GAMESCOPE_RESHADE_EFFECT 'NearBlackMura_Fix.fx'; %command%; DISPLAY=:0 xprop -root -remove GAMESCOPE_RESHADE_EFFECT"
```

## Flatpak
Install vkBasalt from Discover

Drag and drop and choose "Link Here" ``/home/deck/.config/vkBasalt/vkBasalt.conf`` to ``/home/deck/.local/share/flatpak/runtime/org.freedesktop.Platform.VulkanLayer.vkBasalt/x86_64/24.08/{UNIQUE NUMBER}/files/etc/vkBasalt``

If it's not work, try changing the flatpak permission from system settings, or flatseal.

## Toggle Globally
- Install Decky Loader and enable Developer mode
- Install [Bash Shortcut Plugin](https://github.com/SDH-Stewardship/bash-shortcuts/pull/1#issuecomment-2460919165)
- Navigate to Plugin Config → Add Shortcut
- Set it **Name ``Toggle vkBasalt``** | **Command ``sh +x /home/deck/.config/bin/Toggle_vkBasalt.sh``** | ``Does this launch an app?`` **Off** | ``Does this shortcut using flags?``**On**

## Uninstall
- Uninstal [vkBasalt](https://github.com/simons-public/steam-deck-vkbasalt-install)
- And remove this
```
/home/deck/.local/bin/(Delete the "bin" folder, if you never use it)
Toggle_vkBasalt.sh

/home/deck/.local/share/gamescope/reshade/
NearBlackMura_Fix.fx
NearBlackMura_Fix_HDR.fx
NearBlackMura_Fix_AutoHDR.fx
ReShade.fxh
ReShadeUI.fxh
```

<br>

# Credit
- Film Grain Reference - Christian Cann Schuldt Jensen ~ CeeJay.dk
- Lift Gamma Gain Reference - 3an and CeeJay.dk
- [Original Mura Fix](https://www.reddit.com/r/SteamDeck/comments/1aej469/maybe_we_can_correct_mura_like_this_for_now/) Reference
- [Mura fix v2](https://www.reddit.com/r/SteamDeck/comments/1au9blh/updated_fix_for_steamdeck_oled_mura_noise_in/) Reference
- [Steam Deck vkBasalt](https://github.com/simons-public/steam-deck-vkbasalt-install)
