# adwaita-mono-nerd-font

![Screenshot From 2025-03-27 13-03-17](https://github.com/user-attachments/assets/fdcd1d81-4a8d-4d4e-ae60-72895b2c3afd)

This repo contains the files for Nerd Font patched [Adwaita Mono](https://blogs.gnome.org/monster/introducing-adwaita-fonts/) font. 
It has the patched Nerd Font files already so you don't have to run font-forge to create them.

It uses this script to make this happen, using the following repos:
- [Nerd Fonts repo](https://github.com/ryanoasis/nerd-fonts)
- [Adwaita Fonts, not patched](https://gitlab.gnome.org/GNOME/adwaita-fonts)

If you want to do this yourself, you can:
1. Make sure that Adwaita Fonts (not patched) are downloaded into `$adwaita_fonts_dir` before running this.
2. Clone the Nerd Fonts repo, and place the fish script below in the `nerd-fonts` folder and run it.

```fish
#!/usr/bin/env fish

set fonts_dir ~/.local/share/fonts
set adwaita_fonts_dir $fonts_dir/adwaita-fonts
set adwaita_nerd_fonts_dir $fonts_dir/adwaita-nerd-fonts

mkdir -p adwaitamono
mkdir -p adwaitamonopatched

# Find the subfolder within ~/.local/share/fonts/adwaita-fonts/.
set adwaita_font_dir (string trim -- (find $adwaita_fonts_dir/ -mindepth 1 -maxdepth 1 -type d))

if test -z "$adwaita_font_dir"
    echo (set_color red)"Error: No subfolder found in "$adwaita_fonts_dir(set_color normal)
    exit 1
end

# Example usage: You can now use $adwaita_font_dir in subsequent commands.
echo (set_color yellow)"Adwaita font directory: "(set_color normal)"$adwaita_font_dir"

# Example: If you needed to copy files from the font directory to the new directories.
# Example: cp "$adwaita_font_dir/some_font.ttf" adwaitamono/

set adwaita_mono_dir "$adwaita_font_dir/mono"

if not test -d "$adwaita_mono_dir"
    echo (set_color red)"Error: "$adwaita_mono_dir" not found"(set_color normal)
    exit 1
end

echo (set_color green)"Copying AdwaitaMono fonts to "(set_color yellow)"adwaitamono"(set_color green)" directory..."(set_color normal)
cp "$adwaita_mono_dir"/* adwaitamono/

echo (set_color green)"Patching AdwaitaMono fonts to "(set_color yellow)"adwaitamonopatched"(set_color green)" directory..."(set_color normal)
./font-patcher adwaitamono/AdwaitaMono-BoldItalic.ttf --outputdir=adwaitamonopatched
./font-patcher adwaitamono/AdwaitaMono-Italic.ttf --outputdir=adwaitamonopatched
./font-patcher adwaitamono/AdwaitaMono-Bold.ttf --outputdir=adwaitamonopatched
./font-patcher adwaitamono/AdwaitaMono-Regular.ttf --outputdir=adwaitamonopatched

echo (set_color green)"Copying patched fonts to "(set_color yellow)"adwaita-nerd-fonts"(set_color green)" directory..."(set_color normal)
if test -d "$adwaita_nerd_fonts_dir"
    rm -rf "$adwaita_nerd_fonts_dir"
end
mkdir -p $adwaita_nerd_fonts_dirmkdir -p $adwaita_nerd_fonts_dir
cp adwaitamonopatched/* $adwaita_nerd_fonts_dir
```
