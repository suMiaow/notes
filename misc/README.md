# MISC

## oh-my-posh setting

### pwsh

```pwsh
$theme = Get-ChildItem $env:UserProfile\\AppData\\Local\\Programs\\oh-my-posh\\themes\\ | Get-Random
$themeName = $theme.Name;
if ($themeName.Contains('.omp.json')) {
  $themeName = $themeName.SubString(0, $themeName.IndexOf('.'));
  echo "◆ omp theme: $themeName ◆";
  oh-my-posh init pwsh --config $theme.FullName | Invoke-Expression;
} else {
  echo "◆ omp theme: ? ◆";
  oh-my-posh init pwsh | Invoke-Expression;
}
```

### bash

```bash
export PATH="$HOME/.bin:$PATH"

function load_random_theme() {
  local dir=~/.cache/oh-my-posh/themes
  local files=$(ls $dir)
  local file=$(echo $files | cut -d ' ' -f $(($RANDOM%$(echo $files | wc -w) + 1)))
  local posh_theme=$file
  local full_posh_theme=$dir/$file
  echo "◆ omp theme: "${posh_theme%%.*}" ◆"
  eval "$(oh-my-posh init bash --config $full_posh_theme)"
}
load_random_theme
```

