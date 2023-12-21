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
