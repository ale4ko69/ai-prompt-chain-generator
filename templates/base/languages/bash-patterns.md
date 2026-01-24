# Bash / Shell Script Patterns

**Purpose:** Language-specific coding patterns for Bash and shell scripts.

---

## Code Style

### Shebang
**Always start with shebang:**
```bash
#!/usr/bin/env bash
# or
#!/bin/bash
```

**Use `/usr/bin/env bash`** for portability (finds bash in PATH).

### Indentation
**Standard:** 2 or 4 spaces (not tabs)

### Line Length
**Recommendation:** Keep under 80-100 characters

---

## Naming Conventions

### Files
```bash
# kebab-case or snake_case
my-script.sh
install_dependencies.sh
backup-database.sh
```

### Variables
```bash
# snake_case for variables
user_name="John"
max_retries=3
api_base_url="https://api.example.com"
```

### Constants
```bash
# UPPER_SNAKE_CASE for constants (readonly)
readonly MAX_RETRIES=3
readonly API_BASE_URL="https://api.example.com"
readonly LOG_FILE="/var/log/myapp.log"
```

### Functions
```bash
# snake_case for functions
function get_user_by_id() {
    local user_id=$1
    # Implementation
}

# or without 'function' keyword (more portable)
get_user_by_id() {
    local user_id=$1
    # Implementation
}
```

---

## Error Handling

### Exit on Error
```bash
#!/usr/bin/env bash
set -e  # Exit immediately if a command exits with a non-zero status
set -u  # Treat unset variables as an error
set -o pipefail  # Return value of a pipeline is the last command to exit with non-zero status

# Combined (recommended for strict mode)
set -euo pipefail
```

### Trap for Cleanup
```bash
#!/usr/bin/env bash

cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tempfile
    # Other cleanup tasks
}

trap cleanup EXIT  # Run cleanup on script exit
trap cleanup ERR   # Run cleanup on error

# Script body
```

### Error Messages
```bash
#!/usr/bin/env bash

error() {
    echo "ERROR: $*" >&2
    exit 1
}

warn() {
    echo "WARNING: $*" >&2
}

info() {
    echo "INFO: $*"
}

# Usage
if [ ! -f "$config_file" ]; then
    error "Configuration file not found: $config_file"
fi

warn "Deprecated feature used"
info "Starting backup process..."
```

---

## Variables & Quoting

### Variable Declaration
```bash
# Local variables (inside functions)
function process_data() {
    local input_file=$1
    local output_file=$2
    # Function body
}

# Global variables
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "$0")"
```

### Always Quote Variables
```bash
# ✅ Good: Quoted variables
file_name="my file.txt"
cat "$file_name"

# ❌ Bad: Unquoted variables (breaks on spaces)
cat $file_name  # Expands to: cat my file.txt

# ✅ Good: Array expansion
files=("file1.txt" "file2.txt" "file3.txt")
for file in "${files[@]}"; do
    echo "$file"
done
```

### Default Values
```bash
# Use default value if variable is unset or null
name="${1:-default_name}"
port="${PORT:-8080}"

# Assign default value if variable is unset
: "${LOG_LEVEL:=info}"
```

---

## Functions

### Function Definition
```bash
# Preferred: POSIX-compatible syntax
get_user_count() {
    local database=$1
    # Implementation
    echo "$count"
}

# Alternative: Bash-specific syntax
function get_user_count {
    local database=$1
    # Implementation
    echo "$count"
}
```

### Return Values
```bash
# Use echo for returning strings/data
get_timestamp() {
    echo "$(date +%Y-%m-%d_%H-%M-%S)"
}

# Capture function output
timestamp=$(get_timestamp)

# Use exit codes for success/failure
check_file_exists() {
    local file=$1
    if [ -f "$file" ]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

# Check exit code
if check_file_exists "/path/to/file"; then
    echo "File exists"
else
    echo "File not found"
fi
```

---

## Conditionals

### If Statements
```bash
# File tests
if [ -f "$file" ]; then
    echo "File exists"
fi

if [ ! -d "$directory" ]; then
    mkdir -p "$directory"
fi

# String comparisons
if [ "$string1" = "$string2" ]; then
    echo "Strings are equal"
fi

if [ -z "$variable" ]; then
    echo "Variable is empty"
fi

# Numeric comparisons
if [ "$count" -gt 0 ]; then
    echo "Count is greater than zero"
fi

# Use [[ ]] for advanced tests (Bash-specific)
if [[ "$string" =~ ^[0-9]+$ ]]; then
    echo "String is a number"
fi

if [[ "$file" == *.txt ]]; then
    echo "File is a text file"
fi
```

### Case Statements
```bash
case "$action" in
    start)
        echo "Starting service..."
        start_service
        ;;
    stop)
        echo "Stopping service..."
        stop_service
        ;;
    restart)
        echo "Restarting service..."
        stop_service
        start_service
        ;;
    *)
        echo "Unknown action: $action"
        exit 1
        ;;
esac
```

---

## Loops

### For Loop
```bash
# Iterate over files
for file in /path/to/files/*.txt; do
    echo "Processing $file"
done

# Iterate over array
files=("file1.txt" "file2.txt" "file3.txt")
for file in "${files[@]}"; do
    echo "Processing $file"
done

# C-style for loop
for ((i = 0; i < 10; i++)); do
    echo "Iteration $i"
done
```

### While Loop
```bash
# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < input.txt

# Counter-based loop
counter=0
while [ $counter -lt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

---

## Arrays

### Array Declaration
```bash
# Indexed array
files=("file1.txt" "file2.txt" "file3.txt")

# Append to array
files+=("file4.txt")

# Access elements
echo "${files[0]}"  # First element
echo "${files[@]}"  # All elements (space-separated)
echo "${#files[@]}" # Array length

# Iterate over array
for file in "${files[@]}"; do
    echo "$file"
done
```

### Associative Arrays (Bash 4+)
```bash
declare -A user_ages
user_ages[john]=30
user_ages[jane]=25

# Access
echo "${user_ages[john]}"

# Iterate
for name in "${!user_ages[@]}"; do
    echo "$name is ${user_ages[$name]} years old"
done
```

---

## Input/Output

### Reading User Input
```bash
# Read input
read -p "Enter your name: " user_name
echo "Hello, $user_name"

# Read password (hidden input)
read -sp "Enter password: " password
echo

# Read with timeout
read -t 10 -p "Enter value (10s timeout): " value || echo "Timed out"
```

### Redirections
```bash
# Redirect stdout to file
echo "Log message" > output.log

# Append stdout to file
echo "Another message" >> output.log

# Redirect stderr to file
command 2> error.log

# Redirect both stdout and stderr
command > output.log 2>&1
# or (Bash 4+)
command &> output.log

# Read from file
while IFS= read -r line; do
    echo "$line"
done < input.txt

# Here document
cat <<EOF > config.txt
Setting1=Value1
Setting2=Value2
EOF
```

---

## Command Substitution

### Modern Syntax
```bash
# ✅ Preferred: $() syntax
current_date=$(date +%Y-%m-%d)
file_count=$(ls -1 | wc -l)
user_home=$(eval echo ~"$username")
```

### Legacy Syntax (Avoid)
```bash
# ❌ Avoid: Backticks (harder to nest)
current_date=`date +%Y-%m-%d`
```

---

## ShellCheck Compliance

### Install ShellCheck
```bash
# Ubuntu/Debian
sudo apt install shellcheck

# macOS
brew install shellcheck
```

### Run ShellCheck
```bash
shellcheck myscript.sh
```

### Common Directives
```bash
#!/usr/bin/env bash

# Disable specific warnings
# shellcheck disable=SC2086
variable=$unquoted_variable

# Disable for entire file
# shellcheck disable=SC2034
unused_variable="value"
```

---

## Script Structure

### Full Example
```bash
#!/usr/bin/env bash
#
# Script Name: backup-database.sh
# Description: Backs up PostgreSQL database to S3
# Author: Your Name
# Date: 2026-01-21
#

set -euo pipefail

# Constants
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly BACKUP_DIR="/var/backups/postgres"
readonly S3_BUCKET="s3://my-backups"
readonly TIMESTAMP="$(date +%Y-%m-%d_%H-%M-%S)"

# Functions
error() {
    echo "ERROR: $*" >&2
    exit 1
}

info() {
    echo "INFO: $*"
}

cleanup() {
    info "Cleaning up temporary files..."
    rm -f "$temp_file"
}

backup_database() {
    local db_name=$1
    local backup_file="${BACKUP_DIR}/${db_name}_${TIMESTAMP}.sql.gz"

    info "Backing up database: $db_name"

    pg_dump "$db_name" | gzip > "$backup_file" || error "Database backup failed"

    info "Uploading to S3..."
    aws s3 cp "$backup_file" "${S3_BUCKET}/" || error "S3 upload failed"

    info "Backup complete: $backup_file"
}

main() {
    trap cleanup EXIT

    # Check dependencies
    command -v pg_dump >/dev/null 2>&1 || error "pg_dump not found"
    command -v aws >/dev/null 2>&1 || error "aws CLI not found"

    # Create backup directory if it doesn't exist
    mkdir -p "$BACKUP_DIR"

    # Perform backup
    backup_database "myapp"

    info "All backups completed successfully"
}

# Run main function
main "$@"
```

---

## Testing

### BATS (Bash Automated Testing System)
```bash
# test_script.bats
#!/usr/bin/env bats

@test "addition using bc" {
    result=$(echo "2 + 2" | bc)
    [ "$result" -eq 4 ]
}

@test "file exists" {
    touch /tmp/testfile
    [ -f /tmp/testfile ]
    rm /tmp/testfile
}
```

---

## Common Patterns

### Parse Command-Line Arguments
```bash
while [[ $# -gt 0 ]]; do
    case $1 in
        -h|--help)
            show_help
            exit 0
            ;;
        -v|--verbose)
            verbose=true
            shift
            ;;
        -f|--file)
            file="$2"
            shift 2
            ;;
        *)
            error "Unknown option: $1"
            ;;
    esac
done
```

### Retry Logic
```bash
retry() {
    local max_attempts=$1
    shift
    local attempt=1

    until "$@"; do
        if [ $attempt -ge $max_attempts ]; then
            error "Command failed after $max_attempts attempts"
        fi
        warn "Attempt $attempt failed. Retrying in 5 seconds..."
        sleep 5
        ((attempt++))
    done
}

# Usage
retry 3 curl -f https://api.example.com
```

### Parallel Execution
```bash
# Run commands in parallel
for file in *.txt; do
    process_file "$file" &
done

# Wait for all background jobs to complete
wait
```

---

## Key Principles

1. **Always use `#!/usr/bin/env bash`** shebang
2. **Use `set -euo pipefail`** for strict mode
3. **Always quote variables:** `"$variable"`
4. **Use `[[ ]]` for tests** (Bash-specific, more powerful)
5. **Use `local` for function variables**
6. **snake_case for variables/functions**, **UPPER_SNAKE_CASE for constants**
7. **Use `trap` for cleanup**
8. **Check command existence:** `command -v cmd >/dev/null`
9. **Run `shellcheck` on all scripts**
10. **Comment complex logic**

---

## Debugging

### Enable Debug Mode
```bash
#!/usr/bin/env bash
set -x  # Print commands before execution

# or run script with:
bash -x myscript.sh
```

### Selective Debugging
```bash
# Enable debugging for specific section
set -x
complex_operation
set +x  # Disable debugging
```

---

**This template should be used for all Bash/Shell script projects (deployment scripts, automation, CI/CD, system administration).**
