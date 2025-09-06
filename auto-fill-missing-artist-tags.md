# Auto fill missing Artist/Title Tags

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
