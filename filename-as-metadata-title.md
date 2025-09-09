# Filename as metadata title

> Set the title in the metadata of all mp4 files based on the respective filename

```bash
for FILE_PATH in *.mp4; do
    if [ ! -f "$FILE_PATH" ]; then
        continue
    fi

    BASE_NAME=$(basename "$FILE_PATH" .mp4)
    TEMP_FILE="${FILE_PATH}_temp.mp4"

    ffmpeg -i "$FILE_PATH" -c copy -metadata title="$BASE_NAME" "$TEMP_FILE" && mv "$TEMP_FILE" "$FILE_PATH"
done

echo "Metadata title set for all MP4 files in directory"
```


---


```bash
#!/bin/bash

# Loop through directories one level deep
for DIR in */ ; do
    # Ensure it's a directory
    if [ -d "$DIR" ]; then
        # Loop through mp4 files in this subdirectory
        for FILE_PATH in "$DIR"*.mp4; do
            # Check if the file exists
            if [ ! -f "$FILE_PATH" ]; then
                continue
            fi
            
            # Extract basename without the path and extension
            BASE_NAME=$(basename "$FILE_PATH" .mp4)
            
            # Define a temp file path
            TEMP_FILE="${FILE_PATH%.*}_temp.mp4"
            
            # Set the metadata title using ffmpeg and then replace the original file
            ffmpeg -i "$FILE_PATH" -c copy -metadata title="$BASE_NAME" "$TEMP_FILE" && mv -f "$TEMP_FILE" "$FILE_PATH"
        done
    fi
done

echo "Metadata title set for all MP4 files in subdirectories."
```