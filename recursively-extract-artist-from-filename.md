# Recursively extract artist from filename

> Used to extract the first artist name, assuming it sits between the first " - " and second " - "

```bash
#!/bin/sh

SEARCH_DIR="/path/to/dir"
OUTPUT_FILE="artist_names.txt"

cd "$SEARCH_DIR" || exit

> "$OUTPUT_FILE"

find . -type f -name '* - *' | while read -r file; do
    intermediate_string=$(echo "$file" | sed -n 's/.* - \(.*\) - .*/\1/p')

    artist=$(echo "$intermediate_string" | \
             sed -e 's/ x .*//i' \
                 -e 's/ X .*//i' \
                 -e 's/ ft\. .*//i' \
                 -e 's/ feat\. .*//i' \
                 -e 's/ & .*//i' \
                 -e 's/, .*//i' \
                 -e 's/ + .*//i')

    echo "$artist" >> "$OUTPUT_FILE"
done

cd - || exit
```
