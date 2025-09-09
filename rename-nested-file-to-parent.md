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


---


> Rename parent folder to nested mp4

```bash
#!/bin/bash

# Loop through each subdirectory in the current directory
for dir in */; do
    # Check if it's a directory
    if [[ -d "$dir" ]]; then
        # Find the first .mp4 file in the directory
        mp4_file=$(find "$dir" -maxdepth 1 -type f -name '*.mp4' | head -n 1)
        
        # If a .mp4 file is found
        if [[ -n "$mp4_file" ]]; then
            # Get the file's basename without path and extension
            mp4_base_name="$(basename "${mp4_file}" .mp4)"
            
            # Trim the trailing slash from the directory name
            trimmed_dir_name="${dir%/}"
            
            # New directory name will be the name of the .mp4 file
            new_dir_name="${mp4_base_name}"

            # Rename the directory, only if new directory name is different than the original
            if [[ "$trimmed_dir_name" != "$new_dir_name" ]]; then
                # Check if target directory name already exists
                if [[ ! -d "$new_dir_name" ]]; then
                    mv -n "$dir" "$new_dir_name"
                    echo "Renamed directory '$dir' to '$new_dir_name'"
                else
                    echo "Cannot rename '$dir' to '$new_dir_name' because the target directory already exists."
                fi
            fi
        else
            echo "No .mp4 file found in $dir"
        fi
    fi
done
```
