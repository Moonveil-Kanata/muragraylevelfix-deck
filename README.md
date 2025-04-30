# An Ultimate Way to Fix Gray-Level and Mura on Steam Deck

### TL;DR

Average Samsung-panel on Steam Deck have known-issues called Mura Effect, where it has bad gray-uniformity. Resulting, grainy/dirty looks on the screen which can consider looks like a noise or film grain. Causing gradient effects on near-black looks like banding when fading into black.

After certain level of brightness between 35/40% above and certain higher screen hertz, it seems grey level is raised too much. Resulting raised black, more visible mura, and bad transition effects on gradient between black-level and grey is more prominent.

In a nutshell, **you can fix it by set your screen hertz between 47hz-66hz and set your brightness between 35/40%, and disable mura compensation on developer settings**. And you will get the screen where it should be, perfect black and nice gradient on near-black. **But this is not apply with higher brightness.**

This fix will use combination between film grain, lift & gamma, and your mura map. While this is not perfect, atleast it will fix half of the screen issue.

# Usage
You can use Reshadeck, but you will get severe performance hit.

Installation
1. Install vkbasalt locally, follow this guide https://github.com/simons-public/steam-deck-vkbasalt-install
2. Download my custom fix shader and extract
3. Set your screen to 60hz for cleaner image (Or test by yourself every hz, which hertz that has less mura effect)
4. Go to developer settings, disable mura compensation for a perfect black. As the shader already have this fix
5. Done, the shader work globally to all of you vulkan/dx games. You can toggle the effect by open your keyboard by pressing 'steam+x' and 'tab' button

## Troubleshoot
### Emu Deck
For Emu Deck after a couple test logging, it seems .appimage apps keep reading the x86 lib while they're running on x64. This method will make all emudeck emulators work with vkbasalt.

1. Find "/home/deck/.local/share/vulkan/implicit_layer.d/"
2. rename file "vkBasalt.x86.json" to "vkBasalt.x86.json.bak"

### HDR Games
HDR game have pale base rgb and higher mura-effect. So use this launch-command per-game with active HDR compatible games.
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_HDR.conf %command%
```

### Auto HDR Games
For some of you using auto hdr reshade(non vkbasalt) from here https://www.reddit.com/r/SteamDeck/comments/1b4rbd8/auto_hdr_works_on_steam_deck_now/

you can apply this command per-game
```
VKBASALT_CONFIG_FILE=/home/deck/.config/vkBasalt/vkBasalt_AutoHDR.conf %command%
```

## Credit
- Film Grain Reference - Christian Cann Schuldt Jensen ~ CeeJay.dk
- Lift Gamma Gain Reference - 3an and CeeJay.dk
- Original mura fix reference - https://www.reddit.com/r/SteamDeck/comments/1aej469/maybe_we_can_correct_mura_like_this_for_now/
- Mura fix v2 reference - https://www.reddit.com/r/SteamDeck/comments/1au9blh/updated_fix_for_steamdeck_oled_mura_noise_in/
- vkBasalt local on Deck https://github.com/simons-public/steam-deck-vkbasalt-install
