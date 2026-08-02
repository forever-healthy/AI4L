---
name: er-combiner
description: AI4L - combine all QA files of an ER using bash (lossless)
model: sonnet
color: orange
version: 26.8.2
---

# AI4L - Bash Agent to Combine all QA files of an ER into a single file

* Set [er_filename] to $ARGUMENTS

* Report `COMBINER: Work on QA files for [er_filename]`

Run this bash script (substituting [er_filename], [creation_dir], and [trash_dir] for their actual values):

```bash
#!/usr/bin/env bash
set -euo pipefail

ER_FILE="[er_filename]"
CREATION_DIR="[creation_dir]"
TRASH_DIR="[trash_dir]"

# Derive base name by stripping the .md extension (keeps the _ER suffix)
base="${ER_FILE%.md}"

# Find all timestamped QA files, sorted descending by name (date suffix is lexicographically sortable)
mapfile -t qa_files < <(ls -r "${CREATION_DIR%/}/${base}_QA_"*.md 2>/dev/null || true)

count=${#qa_files[@]}
if [[ $count -eq 0 ]]; then
  echo "Error: no timestamped QA files found for $ER_FILE"
  exit 1
fi

echo "Found $count QA file(s):"
for f in "${qa_files[@]}"; do echo "  $f"; done

# Sum audit_duration (HH:MM) across all QA files
total_min=0
for f in "${qa_files[@]}"; do
  d=$(awk '/^audit_duration:/{sub(/^audit_duration:[[:space:]]*/,""); gsub(/[[:space:]"]/,""); print; exit}' "$f")
  if [[ "$d" =~ ^([0-9]+):([0-9]+)$ ]]; then
    total_min=$(( total_min + 10#${BASH_REMATCH[1]} * 60 + 10#${BASH_REMATCH[2]} ))
  fi
done
audit_duration=$(printf '%02d:%02d' $((total_min / 60)) $((total_min % 60)))

# Latest is first after reverse sort
latest="${qa_files[0]}"

# New filename: base already ends in _ER, so this yields ..._ER_QA.md (no date suffix)
new_filename="${base}_QA.md"
new_filepath="${CREATION_DIR%/}/${new_filename}"

# Copy the latest QA file verbatim as the base
cp "$latest" "$new_filepath"
echo "Base: $latest -> $new_filepath"

# Update frontmatter: set audit_filename, audit_iterations, audit_duration
awk -v fn="$new_filename" -v itr="$count" -v dur="$audit_duration" '
  BEGIN { in_fm=0; done_fm=0; found_fn=0; found_itr=0; found_dur=0 }
  /^---/ && !done_fm {
    if (!in_fm) { in_fm=1; print; next }
    else {
      if (!found_fn) print "audit_filename: " fn
      if (!found_itr) print "audit_iterations: " itr
      if (!found_dur) print "audit_duration: \"" dur "\""
      done_fm=1; print; next
    }
  }
  in_fm && /^audit_filename:/ { print "audit_filename: " fn; found_fn=1; next }
  in_fm && /^audit_iterations:/ { print "audit_iterations: " itr; found_itr=1; next }
  in_fm && /^audit_duration:/ { print "audit_duration: \"" dur "\""; found_dur=1; next }
  { print }
' "$new_filepath" > "${new_filepath}.tmp" && mv "${new_filepath}.tmp" "$new_filepath"

# Extract pass_rate from summary table and normalize to 2 decimal places
pass_rate=$(awk '/\*\*Pass Rate\*\*/{
  if (match($0, /[0-9]+\.?[0-9]*%/)) {
    raw = substr($0, RSTART, RLENGTH - 1)
    printf "%.2f%%", raw + 0
    exit
  }
}' "$new_filepath")

# Append Issues+Fixes sections from older files in descending order (skip index 0 = latest)
for ((i=1; i<count; i++)); do
  qa_file="${qa_files[$i]}"
  echo "Appending from: $qa_file"
  # Extract from the first ## Issues line to end of file
  awk '/^## Issues /{found=1} found{print}' "$qa_file" >> "$new_filepath"
  echo "" >> "$new_filepath"
done

# Move all timestamped QA files to trash
mkdir -p "$TRASH_DIR"
for qa_file in "${qa_files[@]}"; do
  mv "$qa_file" "$TRASH_DIR/"
  echo "Trashed: $qa_file"
done

echo ""
echo "QA file: $new_filename"
echo "audit_iterations: $count"
echo "audit_duration: $audit_duration"
echo "pass_rate: $pass_rate"
```

* Run the script
* Return the last 4 lines of output as the result

* Report `COMBINER: Done with [topic]`
