# Remove timestamps

> Remove timestamps from files and folders

```bash
#!/bin/bash

for file in [0-9][0-9]-[0-9][0-9]-[0-9][0-9][0-9][0-9]*; do
    if ! [[ $file =~ ^[0-9][0-9]-[0-9][0-9]-[0-9]{4}\ [0-9]{1,2}#[0-9]{2}\ (AM|PM)\ -\  ]]; then
        continue
    fi
    
    newname=$(echo "$file" | sed -E 's/^[0-9]{2}-[0-9]{2}-[0-9]{4} [0-9]{1,2}#[0-9]{2} (AM|PM) - //')
    mv -- "$file" "$newname"
done

echo "Files have been renamed."
```
