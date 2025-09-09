# Metadata Stuff

> Search the current directory for MP4 files with the specified artist's name and append the paths to the output file.

```bash
#!/bin/bash

ARTIST_NAME="Artist Name"
OUTPUT_FILE="artist_mp4_paths.txt"

> "$OUTPUT_FILE"

find . -type f -iname "*.mp4" -exec bash -c 'ffmpeg -i "$1" 2>&1 | grep -q "$0" && echo "$1"' "$ARTIST_NAME" '{}' \; >> "$OUTPUT_FILE"

echo "All paths have been stored in $OUTPUT_FILE"
```


---


> Auto fill missing Artist/Title Tags based on existing tags

```bash
#!/bin/bash

DIRECTORY="/path/to/dir"
LOGFILE="/path/to/dir/logfile.log"

artist_like_attributes=( "ARTISTS" "ARTIST SORT ORDER" "ALBUM ARTIST" "ALBUM ARTIST SORT ORDER" )
title_like_attributes=( "ALBUM" )

find "$DIRECTORY" -type f \( -iname "*.flac" -o -iname "*.mp3" -o -iname "*.opus" \) | while IFS= read -r file; do    
    echo "Processing file: $file" >> $LOGFILE

    artist=$(ffprobe -v error -show_entries format_tags=artist -of default=noprint_wrappers=1:nokey=1 "$file")
    title=$(ffprobe -v error -show_entries format_tags=title -of default=noprint_wrappers=1:nokey=1 "$file")
    
    if [ -n "$artist" ]; then
        metadata_to_set=()
        
        for attribute in "${artist_like_attributes[@]}"; do
            value=$(ffprobe -v error -show_entries "format_tags=$attribute" -of default=noprint_wrappers=1:nokey=1 "$file")
            
            if [ -z "$value" ]; then
                metadata_to_set+=(-metadata "$attribute=$artist")
            fi
        done
        
        for attribute in "${title_like_attributes[@]}"; do
            value=$(ffprobe -v error -show_entries "format_tags=$attribute" -of default=noprint_wrappers=1:nokey=1 "$file")
            
            if [ -z "$value" ]; then
                metadata_to_set+=(-metadata "$attribute=$title")
            fi
        done

       if [ ${#metadata_to_set[@]} -ne 0 ]; then
            temp_file=$(dirname "$file")/temp_$(basename "$file")

            ffmpeg -i "$file" -c copy "${metadata_to_set[@]}" -y "$temp_file" && mv -f "$temp_file" "$file"
            
            if [ $? -eq 0 ]; then
                echo "Successfully updated: $file" >> $LOGFILE
            else
                echo "FFmpeg failed for: $file" >> $LOGFILE
            fi
        fi
    fi
done
```


---


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


> Set the title in the metadata of all mp4 files based on the respective filename, recursively

```bash
#!/bin/bash

for DIR in */ ; do
    if [ -d "$DIR" ]; then
        for FILE_PATH in "$DIR"*.mp4; do
            if [ ! -f "$FILE_PATH" ]; then
                continue
            fi
            
            BASE_NAME=$(basename "$FILE_PATH" .mp4)
            TEMP_FILE="${FILE_PATH%.*}_temp.mp4"
            
            ffmpeg -i "$FILE_PATH" -c copy -metadata title="$BASE_NAME" "$TEMP_FILE" && mv -f "$TEMP_FILE" "$FILE_PATH"
        done
    fi
done

echo "Metadata title set for all MP4 files in subdirectories."
```


---


> Prefix files with artist, if any

```bash
#!/bin/bash

TARGET_DIR="."
SAVEIFS=$IFS
IFS=$(echo -en "\n\b")

# Function to extract artist name using ffprobe (part of ffmpeg suite)
extract_artist() {
  ffprobe -v error -show_entries format_tags=artist -of default=noprint_wrappers=1:nokey=1 "$1"
}

# Function to rename the file and its parent directory
rename_with_artist() {
  local file_path="$1"
  local artist_name="$2"
  local parent_dir
  local filename

  # Extract the parent directory and the filename from the file path
  parent_dir=$(dirname "$file_path")
  filename=$(basename "$file_path")

  # Replace any forward slashes in artist name to avoid directory traversal
  artist_name=$(echo "$artist_name" | sed 's/\//_/g')

  if [[ ! -z "$artist_name" ]]; then
    mv "$file_path" "${parent_dir}/${artist_name} - ${filename}"
    mv "$parent_dir" "${parent_dir%/*}/${artist_name} - ${parent_dir##*/}"
  fi
}

find "$TARGET_DIR" -type f -iname "*.mp4" | while read -r file; do
  echo "----------------------"
  echo "Processing file: $file"
  artist=$(extract_artist "$file")
  
  if [[ ! -z "$artist" ]]; then
    echo "Artist found: $artist - Renaming file and directory..."
    rename_with_artist "$file" "$artist"
  else
    echo "No artist found for file: $file"
  fi
done

IFS=$SAVEIFS
echo "All files and directories have been processed."
```
