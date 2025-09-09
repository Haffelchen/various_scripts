# Find short Videos

```bash
#!/bin/bash

DURATION_THRESHOLD_SECONDS=180
IGNORED_FOLDERS=("backdrops")
OUTPUT_FILE="short_video_dirs.txt"

> "$OUTPUT_FILE"

find . -type f -name "*.mp4" | while read -r file; do
    parent_dir=$(dirname "$file")

    for ignored_folder in "${IGNORED_FOLDERS[@]}"; do
        if [[ "$parent_dir" == *"$ignored_folder"* ]]; then
            continue 2
        fi
    done

    length=$(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "$file")
    length=$(printf "%.0f\n" "$length")

    if [ "$length" -lt "$DURATION_THRESHOLD_SECONDS" ]; then
        grep -qxF "$parent_dir" "$OUTPUT_FILE" || echo "$parent_dir" >> "$OUTPUT_FILE"
    fi
done

echo "Operation complete. The list of parent directories has been saved to $OUTPUT_FILE."
```
