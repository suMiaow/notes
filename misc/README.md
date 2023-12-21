# MISC

## oh-my-posh setting

### pwsh

```pwsh
$theme = Get-ChildItem $env:UserProfile\\AppData\\Local\\Programs\\oh-my-posh\\themes\\ | Get-Random
$themeName = $theme.Name;
$themeName = $themeName.SubString(0, $themeName.IndexOf('.'));
echo "hello! today's lucky theme is: $themeName :)"
oh-my-posh --init --shell pwsh --config $theme.FullName | Invoke-Expression
```

### bash

```bash
export PATH="$HOME/.bin:$PATH"

function get_random_theme() {
  local dir=~/.cache/oh-my-posh/themes
  local files=$(ls $dir)
  local file=$(echo $files | cut -d ' ' -f $(($RANDOM%$(echo $files | wc -w) + 1)))
  posh_theme=$file
  full_posh_theme=$dir/$file
}
get_random_theme
echo "hello! today's lucky theme is: "${posh_theme%%.*} ":)"
eval "$(oh-my-posh init bash --config $full_posh_theme)"
```
