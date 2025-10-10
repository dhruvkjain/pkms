[[index]]

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

