# Regex Stuff

> Remove everything from first `?` (including) in filenames recursively

```bash
#!/bin/bash

START_DIR="."

find "$START_DIR" -type f -print0 | while IFS= read -r -d '' file; do
    dir=$(dirname "$file")
    base=$(basename "$file")
    new_base="${base%%\?*}"
    
    if [[ "$base" != "$new_base" ]]; then
        mv -n "$file" "${dir}/${new_base}"
    fi
done

echo "File renaming complete."
```

---

> Extract from strings in file

```bash
grep -oP "href='\K[^']+?(?=')" "Bookmarks.html" > urls.txt
grep -oP "HREF=\"\K[^\"]+?(?=\")" "Bookmarks2.html" > urls2.txt
```

---

> Replace something in dir name

```bash
search_string="REPLACE ME - "
replace_string=""

for d in */ ; do
    d="${d%/}"
    
    if [[ "$d" == *"$search_string"* ]]; then
        new_d="${d/$search_string/$replace_string}"
        
        mv -- "$d" "$new_d"
    fi
done
```

---

> Replace anywhere, case insensitive

```bash
for f in *; do
    if [ -f "$f" ]; then
        newname=$(echo "$f" | sed 's/some\.string/A new String/gi')

        mv -- "$f" "$newname"
    fi
done
```

---

> Replace something at beginning of filename

```bash
for f in *; do
    if [ -f "$f" ]; then
        newname=$(echo "$f" | sed 's/^some\.string/A new String/')
        mv -- "$f" "$newname"
    fi
done
```

---

> Replace anywhere in filename

```bash
for f in *; do
    if [ -f "$f" ]; then
        newname=$(echo "$f" | sed 's/BEFORE/After/gi')
        mv -- "$f" "$newname"
    fi
done
```


---


> Extract content between second last `_` and last `_` assuming theres some resolution classifier after the last underscore

```bash
#!/bin/bash

OUTPUT_FILE="extracted_content.txt"

> "$OUTPUT_FILE"

for dir in */; do
    dir="${dir%/}"

    if [[ $dir =~ _([^_]+)_(144|240p|360p|480p|720p|1080p|1440p|2160p)$ ]]; then
        echo "${BASH_REMATCH[1]}" >> "$OUTPUT_FILE"
    fi
done

echo "Extraction complete. Content saved to $OUTPUT_FILE."
```


---


> Extract content before first occurrence of ` - `

```bash
#!/bin/bash

OUTPUT_FILE="extracted_content.txt"

> "$OUTPUT_FILE"

for dir in */; do
    dir="${dir%/}"

    if [[ $dir =~ ^([^\-]+)\ - ]]; then
        echo "${BASH_REMATCH[1]}" >> "$OUTPUT_FILE"
    fi
done

echo "Extraction complete. Content saved to $OUTPUT_FILE."
```

---


> Replace first 4 characters

```bash
for f in * ; do
    if [[ -f "$f" ]]; then
        new_f="${f:4}"
        mv -- "$f" "$new_f"
    fi
done
```
