# Remove N chars from name

```bash
#!/bin/bash

TARGET_DIR="."
REMOVE_N_CHARS=8

for dir in "$TARGET_DIR/*/"; do
    dir="${dir%/}"

    new_dir="${dir:$REMOVE_N_CHARS}"

    mv -- "$dir" "$new_dir"
done
```

---

```bash
#!/bin/bash

TARGET_DIR="."
REMOVE_N_CHARS=8

generate_unique_dir_name() {
    local base_name=$1
    local counter=2
    local new_name

    while true; do
        new_name="${base_name}_${counter}"
        if [[ ! -d "$new_name" ]]; then
            echo "$new_name"
            break
        fi
        ((counter++))
    done
}

for dir in "$TARGET_DIR/*/"; do
    dir="${dir%/}"
    new_dir="${dir:$REMOVE_N_CHARS}"

    if [[ -d "$new_dir" ]]; then
        new_dir=$(generate_unique_dir_name "$new_dir")
    fi

    mv -- "$dir" "$new_dir"
done
```
