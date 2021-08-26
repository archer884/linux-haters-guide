# Chapter 3: Fonts

There's no way around this: fonts are a damnable pain in the ass on Linux. How do you install them? I don't know. What's more, you won't actually know whether or not you DO have a font installed. The fonts application sucks, so you're shit out of luck there. You know what you can do? You can run the following command:

```shell
fc-list
```

If a font shows up in the list, at least your computer is aware of its existence. The list's format is dog shit, and this problem is only exacerbated by modern editors and their refusal to use anything other than the editor itself to configure the editor, so you'll never know which font you're actually using unless it has clown faces for letters, BUT....

```
/usr/share/fonts/truetype/lato/Lato-Medium.ttf: Lato,Lato Medium:style=Medium,Regular
/usr/share/fonts/opentype/fira/FiraMono-Medium.otf: Fira Mono,Fira Mono Medium:style=Medium,Regular
/usr/share/fonts/opentype/fira/firasanscompressed-regular.otf: Fira Sans Compressed:style=Regular
/usr/share/fonts/truetype/lato/Lato-SemiboldItalic.ttf: Lato,Lato Semibold:style=Semibold Italic,Italic
/usr/share/fonts/truetype/dejavu/DejaVuSerif-Bold.ttf: DejaVu Serif:style=Bold
```

See that text after the first colon? That's the actual name of the font. If you want to use it in VS Code, that's what you need to enter into the config file. Speaking of config files, here are sample font configs for Visual Studio Code and Alacritty.

```javascript
// Visual Studio Code font configuration
{
    "editor.fontLigatures": true,
    "editor.fontFamily": "'Cascadia Code', 'Droid Sans Mono', 'monospace', monospace, 'Droid Sans Fallback'",
    "terminal.integrated.fontFamily": "'CaskaydiaCove Nerd Font'"
}
```

> Note: Your machine may be convinced that `CaskaydiaCove Nerd Font` is *not* monospace. In that case, try `CaskaydiaCove Nerd Font Mono` because... reasons. I have no idea why this would help. The original *should* be monospace in the first place, but maybe that varies from one version of the font to the next.

```yaml
# Alacritty font configuration
font:
  normal:
    family: 'CaskaydiaCove Nerd Font'
```
