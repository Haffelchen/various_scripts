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


---


> Find folders with prefixes from a file, move into target, nested in folder called as the prefix currently being processed, case insensitive


```bash
#!/bin/bash

SOURCE_DIR="/path/to/source/dir"
TARGET_DIR="/path/to/target/dir"
LIST_FILE="move_with_prefix.txt"

while IFS= read -r line || [[ -n "$line" ]]; do
    echo "----------------------"
    echo "Processing line: $line"

    TARGET_SUBDIR="$TARGET_DIR/$line"
    if [ ! -d "$TARGET_SUBDIR" ]; then
        mkdir -p "$TARGET_SUBDIR"
    fi

    find "$SOURCE_DIR" -maxdepth 1 -type d -iname "$line - *" | while read -r src_dir; do
        echo "Moving '$src_dir' to '$TARGET_SUBDIR'"
        mv "$src_dir" "$TARGET_SUBDIR/"
    done
done < "$LIST_FILE"

echo "Directories have been processed and moved."
```


---


> Find all folders that have either string in their name, case insensitive

```bash
#!/bin/bash

SOURCE_DIR="/path/to/dir"
STRINGS_FILE="find.txt"
OUTPUT_FILE="matches.txt"

> "$OUTPUT_FILE"

while IFS= read -r line || [[ -n "$line" ]]; do
    echo "-------------------------------"
    echo "Processing line $line"
    echo "String: $line" >> "$OUTPUT_FILE"
    
    find "$SOURCE_DIR" -type d -iname "*$line*" -exec echo "  - {}" \; >> "$OUTPUT_FILE"
done < "$STRINGS_FILE"

echo "Search is complete. Results are saved in $OUTPUT_FILE."
```


---


> Find folders containing particular strings from a file (one by one), move into target, nested in folder called as the string currently being processed, case insensitive

```bash
#!/bin/bash

# Set the path to the file with strings, the source directory, and the target directory
SOURCE_DIR="/path/to/source/dir"
TARGET_DIR="/path/to/target/dir"
STRINGS_FILE="find.txt"

while IFS= read -r line || [[ -n "$line" ]]; do
    echo "----------------------"
    echo "Processing line $line"
    
    # Create the target subdirectory if it doesn't exist
    TARGET_SUBDIR="$TARGET_DIR/$line"
    if [ ! -d "$TARGET_SUBDIR" ]; then
        mkdir -p "$TARGET_SUBDIR"
    fi
    
    # Search for and move directories containing the string, case-insensitive, depth of 1
    find "$SOURCE_DIR" -maxdepth 1 -type d -iname "*$line*" | while read -r src_dir; do
        echo "Moving '$src_dir' to '$TARGET_SUBDIR'"
        mv "$src_dir" "$TARGET_SUBDIR/"
    done
done < "$STRINGS_FILE"

echo "Move operation is complete."
```
