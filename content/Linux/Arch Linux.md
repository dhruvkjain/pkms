[[Linux/index|Linux/index]]

### Cleanup Routine Commands:
pacman, yay
```shell
sudo pacman -Rns $(pacman -Qdtq)   # remove repo orphans
yay -Rns $(yay -Qdtq)              # remove AUR orphans
sudo paccache -r                   # trim repo package cache
yay -Sc                            # trim yay build cache
```

cargo:
```shell
cargo cache
cargo cache -a
```

uv:
```shell
uv cache clean
```

python venv:
```shell
pip cache purge
```

list the size of folders:
```shell
du -h . | sort -hr | head -20
```

cache:
```shell
rm -rf ~/.cache/BraveSoftware/Brave-Browser/Default
rm -rf ~/.cache/kioexec
rm -rf ~/.cache/yay
rm -rf ~/.cache/go-build
```
