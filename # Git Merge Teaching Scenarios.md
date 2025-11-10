# Git Merge Teaching Scenarios

## Initial Setup

```bash
# Initialize the repository
git init git-merge-demo
cd git-merge-demo

# Create initial file
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, World!\n";
?>
EOF

# Initial commit
git add hello.php
git commit -m "Initial commit: Hello World in PHP"
```

---

## Scenario 1: Easy Merge (No Conflicts)

**Situation:** Two developers work on different parts of the file.

### Developer A - Adds a greeting function

```bash
# Developer A creates a branch
git checkout -b feature/greeting-function

# Modify hello.php
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application

function greet($name) {
    return "Hello, $name!\n";
}

echo "Hello, World!\n";
?>
EOF

git add hello.php
git commit -m "Add greeting function"
```

### Developer B - Adds a footer

```bash
# Switch back to main and create Developer B's branch
git checkout main
git checkout -b feature/add-footer

# Modify hello.php
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, World!\n";

// Footer
echo "\n--- End of Program ---\n";
?>
EOF

git add hello.php
git commit -m "Add program footer"
```

### Merge (Easy - No Conflict)

```bash
# Merge Developer A's work first
git checkout main
git merge feature/greeting-function
# This works fine!

# Merge Developer B's work
git merge feature/add-footer
# Git automatically merges! No conflict because changes are in different locations
```

**Result:** Git combines both changes automatically.

**Final hello.php:**
```php
<?php
// Simple Hello World Application

function greet($name) {
    return "Hello, $name!\n";
}

echo "Hello, World!\n";

// Footer
echo "\n--- End of Program ---\n";
?>
```

---

## Scenario 2: Conflict Merge

**Situation:** Two developers modify the same line.

### Reset for Conflict Scenario

```bash
# Start fresh from initial commit
git checkout main
git reset --hard HEAD~2  # Remove previous merges
# Or start from a clean slate
```

### Developer A - Personalizes greeting

```bash
git checkout -b feature/personalized-greeting

cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, Alice! Welcome to PHP!\n";
?>
EOF

git add hello.php
git commit -m "Personalize greeting for Alice"
```

### Developer B - Adds enthusiasm

```bash
git checkout main
git checkout -b feature/enthusiastic-greeting

cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, World! This is amazing!\n";
?>
EOF

git add hello.php
git commit -m "Add enthusiasm to greeting"
```

### Merge (Conflict!)

```bash
# Merge Developer A's work first
git checkout main
git merge feature/personalized-greeting
# Success!

# Now merge Developer B's work
git merge feature/enthusiastic-greeting
# CONFLICT! Both modified the same line
```

**Git will show:**
```
Auto-merging hello.php
CONFLICT (content): Merge conflict in hello.php
Automatic merge failed; fix conflicts and then commit the result.
```

**The conflicted hello.php will look like:**
```php
<?php
// Simple Hello World Application
<<<<<<< HEAD
echo "Hello, Alice! Welcome to PHP!\n";
=======
echo "Hello, World! This is amazing!\n";
>>>>>>> feature/enthusiastic-greeting
?>
```

### Resolving the Conflict

**Option 1: Choose Developer A's version**
```bash
# Edit hello.php to keep only Alice's version
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, Alice! Welcome to PHP!\n";
?>
EOF

git add hello.php
git commit -m "Merge feature/enthusiastic-greeting: kept personalized greeting"
```

**Option 2: Choose Developer B's version**
```bash
# Edit hello.php to keep only the enthusiastic version
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, World! This is amazing!\n";
?>
EOF

git add hello.php
git commit -m "Merge feature/enthusiastic-greeting: kept enthusiastic greeting"
```

**Option 3: Combine both (best solution)**
```bash
# Combine both improvements
cat > hello.php << 'EOF'
<?php
// Simple Hello World Application
echo "Hello, Alice! Welcome to PHP! This is amazing!\n";
?>
EOF

git add hello.php
git commit -m "Merge feature/enthusiastic-greeting: combined both greetings"
```

---

## Teaching Points

### Easy Merge Teaches:
- Git can automatically merge changes in different locations
- Branch workflows allow parallel development
- The importance of working on separate features

### Conflict Merge Teaches:
- What causes merge conflicts (same line edits)
- How to identify conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
- The responsibility of resolving conflicts
- Communication between developers is important
- Testing after conflict resolution

### Commands Students Learn:
- `git branch` and `git checkout -b`
- `git merge`
- `git status` (to check for conflicts)
- `git add` (to mark conflicts as resolved)
- `git commit` (to complete the merge)

---

## Bonus: Preventing Conflicts

Have students discuss strategies:
1. **Pull frequently** from main branch
2. **Communicate** about what files you're editing
3. **Keep branches short-lived**
4. **Make small, focused commits**
5. **Rebase** before merging (advanced topic)