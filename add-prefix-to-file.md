# Prefix

> Add prefix to files

```bash
for f in *; do
    if [ -f "$f" ]; then
        mv "$f" "PREFIX - $f"
    fi
done
```

---

> Add prefix to dirs

```bash
for d in */ ; do
    mv "$d" "PREFIX - $d"
done
```
