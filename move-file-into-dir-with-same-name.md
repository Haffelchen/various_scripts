# Move file into dir with same name

> Move all mp4 files into a dir with the same name as the file

```bash
#!/bin/bash

find . -maxdepth 1 -type f -iname *.mp4 -exec bash -c '
  for file in "$@"; do
    # Extract the base name without the extension
    base="${file%.mp4}"

    # Create a new directory with the base name
    mkdir -p "${base}"

    # Move the MP4 file into the newly created directory
    mv "$file" "${base}/"
  done
' bash {} +
```
