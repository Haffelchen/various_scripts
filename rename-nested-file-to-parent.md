# Rename nested file to parent

> Go through all folders, check if mp4 is in there. If so, rename the mp4 to the same as it's respective parent folder

```bash
#!/bin/bash

for dir in */; do
    if [[ -d "$dir" ]]; then
        dir_name="${dir%/}"
        mp4_file=$(find "$dir" -maxdepth 1 -type f -name '*.mp4' | head -n 1)
        
        if [[ -n "$mp4_file" ]]; then
            new_filename="${dir_name}.mp4"
            
            mv -n "$mp4_file" "${dir}/${new_filename}"
            echo "Renamed '$mp4_file' to '${dir}/${new_filename}'"
        else
            echo "No .mp4 file found in $dir"
        fi
    fi
done
```
