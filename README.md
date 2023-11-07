# dotfiles
Dotfiles of Björn Nilsved

## Excluding `node_modules` from MacOS TimeMachine

Check status in different directories:

```sh
sudo find -E /Users -type d -name node_modules -regex "/Users/[^/]+/(Code|Sites)/.*" -prune -exec tmutil isexcluded "{}" \;
sudo find -E /Users -type d -name node_modules -prune -exec tmutil isexcluded "{}" \;
```

Inside `~/Code` and `~/Sites` directories, ask if each `node_modules` should be excluded:

```sh
sudo find -E /Users -type d -name node_modules -regex "/Users/[^/]+/(Code|Sites)/.*" -prune -ok tmutil addexclusion "{}" \;
```
