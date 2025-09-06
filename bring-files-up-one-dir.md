# Bring files up one dir

> Bring files up by one level from their nested dirs

```bash
find . -mindepth 2 -type f \( -name "*.mkv" -o -name "*.mp4" -o -name "*.avi" \) -exec sh -c '
  for file do
    mv -- "$file" "$(dirname "$file")/.."
  done
' sh {} +
```
