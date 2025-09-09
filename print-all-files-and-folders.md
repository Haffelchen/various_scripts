# Print all files and folders

> Save all file and folder names in a directory to a file

```bash
#!/bin/bash

OUTPUT_FILE="directory_contents.txt"

> "$OUTPUT_FILE"

echo "Listing all files and folders in current dir"

for item in *; do
    if [ -f "$item" ]; then
        echo "$(basename "$item")" >> "$OUTPUT_FILE"
    elif [ -d "$item" ]; then
        echo "$(basename "$item")" >> "$OUTPUT_FILE"
    fi
done

echo "Contents have been written to $OUTPUT_FILE."
```

---

> Get all file and folder names in a directory

```bash
#!/bin/bash

echo "Listing all files and folders in current dir"

for item in *; do
    if [ -f "$item" ]; then
        echo "$(basename "$item")"
    elif [ -d "$item" ]; then
        echo "$(basename "$item")"
    fi
done
```
