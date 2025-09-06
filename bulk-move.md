# Bulk Move

```bash
#!/bin/bash

INPUT_FILE="folders_and_files_to_move.txt"
DESTINATION_DIR="/path/to/dir"

while read -r dir; do
    mv "$dir" "$DESTINATION_DIR"
done < "$INPUT_FILE"

echo "All specified directories have been moved to '$DESTINATION_DIR'"
```
