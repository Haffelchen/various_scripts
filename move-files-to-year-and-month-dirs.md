# Move files to year and month dirs

> Extract date from folder name and move into year and month separated sub dirs

```bash
#!/bin/bash

for dir in */; do
    if [[ -d "$dir" ]]; then
        if [[ $dir =~ ([0-9]{4}-[0-9]{2}-[0-9]{2}) ]]; then
            date=${BASH_REMATCH[1]}
            year=${date:0:4}
            month=${date:5:2}

            mkdir -p "$year/$month"
            mv "$dir" "$year/$month/"
        else
            echo "The directory '$dir' does not contain a valid date."
        fi
    fi
done
```
