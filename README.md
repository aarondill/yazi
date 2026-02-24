# [Yazi](https://github.com/sxyazi/yazi) configuration

My personal configuration for [Yazi](https://github.com/sxyazi/yazi).

```shell
> git clone --recurse-submodules https://github.com/aarondill/yazi ~/.config/yazi
> cd ~/.config/yazi/ && ya pkg install
> yazi
```

You may want to add the following to your shell configuration. This will:

- Install Yazi plugins if they are not already installed
- create a file to store the new directory
- Open Yazi
- move to the new directory if it changed

```bash
function yazi() {
  local _installed_file=${XDG_CONFIG_HOME:-$HOME/.config}/yazi/.plugins-installed
  if ! [ -f "$_installed_file" ]; then
    echo "Installing yazi plugins..."
    ya pkg install && touch "$_installed_file"
  fi
  local tmp cwd code=0
  tmp="$(mktemp -t "yazi-cwd.XXXXXX")"
  command yazi "$@" --cwd-file="$tmp" || code=$?
  IFS= read -r -d '' cwd <"$tmp"
  [ "$cwd" != "$PWD" ] && [ -d "$cwd" ] && builtin cd -- "$cwd" || true
  rm -f -- "$tmp"
  return "$code"
}
```
