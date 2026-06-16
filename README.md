# Astrodata Daily Exercises

These exercises are designed to mimic the tasks you will perform every day when working with a terminal.

## Part 1: File & Folder Foundation

### Exercise 1: Create your folders

**Task**
Create a folder named `training/` in your current directory, and inside it, create another folder named `linux_discovery/`. **Stay in your current directory for now.**

**Hint**
Use `mkdir -p path/to/folder` to create parent and child directories in one go.

**Solution**

```bash
mkdir -p ./training/linux_discovery/

```

### Exercise 2: Create configuration files

**Task**
Still from your current directory, create an empty file named `.env` and another named `requirements.txt` **inside** the `training/linux_discovery/` folder.

**Solution**

```bash
touch training/linux_discovery/.env training/linux_discovery/requirements.txt

```

### Exercise 3: Write into a file

**Task**
Add the line `pandas==2.1.0` to the `requirements.txt` file you just created, without leaving your current directory.

**Hint**
Use the redirection operator `>>` and specify the full path to the file.

**Solution**

```bash
echo "pandas==2.1.0" >> training/linux_discovery/requirements.txt

```

### Exercise 4: Move into your workspace

**Task**
Now, move into the folder `training/linux_discovery/` and verify that your files are there.

**Solution**

```bash
cd ./training/linux_discovery/
ls -a

```

### Exercise 5: Copy and Move files

**Task**
You want to secure your configuration.

1. Copy your `.env` file to a new file named `.env.backup`.
2. Create a folder named `config/` and move your `requirements.txt` into it.

**Hint**
Use `cp` for copying and `mv` for moving/renaming.

**Solution**

```bash
# 1. Copy
cp .env .env.backup
# 2. Create folder and move
mkdir config
mv requirements.txt config/

```

---

## Part 2: The Data Scientist Toolbox

### Exercise 6: Project Audit with `tree`

**Setup**
Create a realistic project structure to audit within your current directory:

```bash
mkdir -p project_audit/{data,src,notebooks,.git}
touch project_audit/README.md project_audit/src/main.py project_audit/data/raw.csv
cd project_audit/

```

**Task**
Generate a visual tree of the `project_audit` folder but exclude the `.git/` directory. Save the result into a file named `audit_structure.txt`.

**Hint**
Use `tree -I ".git"`.

**Solution**

```bash
tree -I ".git" > audit_structure.txt

```

### Exercise 7: Inspecting "Big Data" with `less`

**Setup**
First, generate a dummy CSV file with 20 columns and 100 rows of data to test with:

```bash
# Generate header (col_1, col_2, ...)
echo $(seq -f "col_%g" -s, 1 20) > big_data.csv
# Add 100 rows of dummy data
for i in {1..100}; do echo $(seq -f "data_$i_%g" -s, 1 20) >> big_data.csv; done

```

**Task**
Imagine you have a CSV file with many columns. You want to scroll through it without text wrapping (horizontal scrolling). Use the `big_data.csv` you just created.

**Hint**
Use the `-S` (Chop long lines) flag with `less`.

**Solution**

```bash
# Navigate using arrows. Press 'q' to quit.
less -S big_data.csv

```

### Exercise 8: Creating a Data Dictionary

**Setup**
If you skipped the previous exercise, run this command to generate the test data:

```bash
# Generate header
echo $(seq -f "col_%g" -s, 1 20) > big_data.csv
# Add 3 sample rows
for i in {1..3}; do echo $(seq -f "sample_$i_%g" -s, 1 20) >> big_data.csv; done

```

**Task**
You need to document your dataset. Take the **first 4 lines** (header + 3 rows of data) and **transpose** them (columns become rows) to create a clear `data_dictionary.csv`.

**Hint**
Use `head -n 4` and a pipe into an `awk` command to handle the transposition.

**Solution**

```bash
# This powerful awk command transposes the CSV content
head -n 4 big_data.csv | awk -F, '{
    for (i=1; i<=NF; i++) a[i,NR]=$i; 
    if (NF>max_nf) max_nf=NF
} 
END {
    for (i=1; i<=max_nf; i++) {
        for (j=1; j<=NR; j++) printf "%s%s", a[i,j], (j==NR ? "" : ","); 
        print ""
    }
}' > data_dictionary.csv

```

### Exercise 9: Visualizing History with `tig`

**Setup**
Initialize a new Git repository in a separate folder and create some history:

```bash
mkdir ./tig_discovery && cd ./tig_discovery
git init
# First commit
echo "# Project README" > README.md
git add README.md
git commit -m "docs: initial commit"
# Second commit
echo "This is an important data science project." >> README.md
git add README.md
git commit -m "docs: add description to README"

```

**Task**
You want to see who modified the `README.md` and what they changed. Use the interactive `tig` tool.

**Solution**

```bash
tig README.md
# Use arrows to browse and 'Enter' to see the diff. Press 'q' to exit.

```

### Exercise 10: Editing Configs with `nano`

**Setup**
Ensure you have a local `.env` file to edit:

```bash
echo "API_KEY=12345" > .env

```

**Task**
Open your `.env` file, add `DATABASE_URL=localhost` on a new line, and save it.

**Solution**

```bash
nano .env
# Type your text, then CTRL+O (Enter) to save and CTRL+X to exit.

```

### Exercise 11: Smart Data Filtering with `grep`

**Setup**
Create a dummy log file with some informational and error messages:

```bash
echo "2026-04-22 10:00:01 INFO: Application started" > app.log
echo "2026-04-22 10:00:05 INFO: Loading database driver..." >> app.log
echo "2026-04-22 10:00:10 ERROR: Database connection failed (timeout)!" >> app.log
echo "2026-04-22 10:00:11 INFO: Retrying connection (1/3)..." >> app.log
echo "2026-04-22 10:00:15 INFO: Retrying connection (2/3)..." >> app.log
echo "2026-04-22 10:00:20 ERROR: Critical failure. System shutting down." >> app.log
echo "2026-04-22 10:00:21 INFO: Dump generated at /tmp/dump.bin" >> app.log

```

**Task**
Find the word "ERROR" in `app.log` and display the 2 lines *before* and *after* each match to understand the context.

**Hint**
Use `grep -C 2`.

**Solution**

```bash
# -i for case-insensitive, -C 2 for 2 lines of context
grep -i -C 2 "error" app.log

```
