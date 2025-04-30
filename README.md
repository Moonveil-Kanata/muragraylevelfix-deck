# An Ultimate Way to Fix Gray-Level and Mura on Steam Deck
https://github.com/user-attachments/assets/254a30c3-f0c4-4e98-b25e-6407b6a3cd0f

### TL;DR

Average Samsung-panel on Steam Deck have known-issues called Mura Effect, where it has bad gray-uniformity on certain Hz and brightness. Resulting, grainy/dirty looks on the screen which can consider looks like a noise or film grain. Causing gradient effects on near-black looks like banding when fading into black. **While the current fix provided from valve unfortunately is raising the black level with the mura map keep showing on the black pixel.**

In a nutshell, **you can fix it by set your screen hertz between 47hz-66hz and set your brightness between 35/40%, and disable mura compensation on developer settings**. And you will get the screen where it should be, perfect black and nice gradient on near-black. **But this is not apply with higher brightness.**

This fix will use combination between **film grain for dithering, lift & gamma (only on near-black pixel)** and your **mura map (only on your bright pixel)**. While this is not perfect, atleast it will fix half of the screen issue.

> [!IMPORTANT]
> - Only works in-game, not the Steam UI
> - vkBasalt works only on Vulkan and DX, for openGL, use the gamescope method

# Usage
*You can use Reshadeck, but you will get severe performance hit. While this vkbasalt almost no visible performance hit.

**Installation:**
1. Install vkbasalt locally, follow this guide https://github.com/simons-public/steam-deck-vkbasalt-install
2. Download
3. Extract .fx files to
   ``
   /home/deck/.local/share/gamescope/reshade/Shaders
   ``
   and .conf files to
   ``
   /home/deck/.config/vkBasalt
   ``
5. Go to ``rootfs/tmp/mura`` copy ``xxxx_green.png`` and ``xxxx_red.png`` to ``/home/deck/.local/share/gamescope/reshade/Textures`` and rename it to ``green.png`` and ``red.png``
6. Done, now vkBasalt is running globally

**Usage:**
1. Boot to the game mode
2. Press ``•••`` Disable frame limit and set to 60hz (**For less mura as possible**)
3. Press ``STEAM`` Button → Settings → Developer → Activate ``Disable Mura Compensation`` (**For perfect black**)
4. Enjoy, read troubleshoot for advanced stuff

## Troubleshoot
### Emu Deck
For Emu Deck after a couple test logging, it seems .appimage apps keep reading the x86 lib while they're running on x64. This method will make all emudeck emulators work with vkbasalt.

1. Find "/home/deck/.local/share/vulkan/implicit_layer.d/"
2. rename file "vkBasalt.x86.json" to "vkBasalt.x86.json.bak"

### HDR Games
HDR game have pale base rgb and higher mura-effect. So use this as launch-command per-game with active HDR compatible games.
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_HDR.conf %command%
```

### Auto HDR Games
For some of you using auto hdr reshade(non vkbasalt) from here https://www.reddit.com/r/SteamDeck/comments/1b4rbd8/auto_hdr_works_on_steam_deck_now/

you can apply this command per-game
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_AutoHDR.conf %command%
```

### OpenGL | Gamescope
If you have opengl games
```
bash -c "DISPLAY=:0 xprop -root -f GAMESCOPE_RESHADE_EFFECT 8s -set GAMESCOPE_RESHADE_EFFECT 'NearBlackMura_Fix.fx'; %command%; DISPLAY=:0 xprop -root -remove GAMESCOPE_RESHADE_EFFECT"
```

## Credit
- Film Grain Reference - Christian Cann Schuldt Jensen ~ CeeJay.dk
- Lift Gamma Gain Reference - 3an and CeeJay.dk
- Original mura fix reference - https://www.reddit.com/r/SteamDeck/comments/1aej469/maybe_we_can_correct_mura_like_this_for_now/
- Mura fix v2 reference - https://www.reddit.com/r/SteamDeck/comments/1au9blh/updated_fix_for_steamdeck_oled_mura_noise_in/
- vkBasalt local on Deck https://github.com/simons-public/steam-deck-vkbasalt-install
