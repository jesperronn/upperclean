# upperclean

A powerful cleanup utility for developers that removes external libraries, temporary files, unwanted backups, and runs system cleanup commands across your machine or specific projects.

## Features

- **Multi-level cleanup**: Choose from 4 cleanup levels (Quick, Standard, Deep, Aggressive) based on risk tolerance and time available
- **Works both globally and locally**: Clean entire system or specific software project folders
- **Interactive multi-select**: Choose exactly which items to delete from multi-select menus
- **Risk-aware**: Each cleanup level balances between disk recovery and safety
- **CPU-efficient**: Respects system resources - light operations for daily use, heavier analysis for deep cleaning
- **Comprehensive cleaning**:
  - Removes external dependencies (`node_modules`, Maven repositories, etc.)
  - Cleans temporary files and build artifacts
  - Removes unwanted backups
  - Runs system-level cleanup commands (`docker image prune`, `docker system prune`, etc.)
  - Supports language-specific cleanup (`mvn clean`, `brew cleanup`, etc.)

## What It Cleans

### Local Project Level
- `node_modules/` - Node.js dependencies
- `target/` - Maven build output
- `.gradle/` - Gradle build cache
- `dist/` - Build artifacts
- Backup files (`*.bak`, `*.backup`)
- Temporary files

### System Level
- Docker images and containers
- Package manager caches
- Temporary system files
- Maven local repository (with confirmation)

### High-Risk Operations (Interactive Multi-Select)
Sensitive operations present a multi-select menu where you can choose exactly what to delete:
- Old language/runtime versions (Node, Ruby, Python)
- Large Maven repositories
- Unused Docker volumes
- Orphaned Python virtual environments
- Git repository optimization

## Cleanup Levels

upperclean offers 4 cleanup levels, inspired by real-world cleaning routines (daily, weekly, monthly, yearly). Each level balances **risk** (potential data loss) and **complexity** (CPU/time required to analyze what to clean).

### Quick (Daily Cleaning)
**Risk Level**: ⭐ Minimal | **Complexity**: ⭐ Very Low

Run daily or multiple times per week. Ultra-safe operations with zero risk of data loss.

**Includes**:
- `npm cache clean`, `pip cache purge`, `yarn cache clean`
- `brew cleanup` (just cleanup, not aggressive)
- Docker dangling images prune
- IDE/editor cache cleanup (VS Code, Sublime)
- Vim swap files (`~/.vim/swap`)
- Git garbage collection (project-level)

**Time**: < 1 minute | **Recovery**: 100MB - 500MB typically

---

### Standard (Weekly Cleaning)
**Risk Level**: ⭐⭐ Low | **Complexity**: ⭐⭐ Low

Run weekly as part of normal maintenance. Includes safe system-level cleanup with minimal risk.

**Includes everything from Quick, plus**:
- Docker stopped containers and build cache
- Homebrew aggressive cleanup (`brew cleanup -s`)
- Package manager caches (APT, RubyGems)
- IntelliJ/JetBrains IDE caches
- Xcode derived data (macOS)
- Old log files (> 30 days)
- System temp files cleanup

**Time**: 2-5 minutes | **Recovery**: 500MB - 3GB typically

---

### Deep (Monthly Cleaning)
**Risk Level**: ⭐⭐⭐ Medium | **Complexity**: ⭐⭐⭐ Medium

Run monthly or before major projects. Requires some caution; multi-select prompts for high-risk items.

**Includes everything from Standard, plus**:
- Gradle caches (`~/.gradle/caches`)
- Cargo registry cache (Rust)
- SBT cache
- Android SDK old versions
- System crash dumps
- **Multi-Select**: Old Node/Ruby version cleanup (nvm, rbenv)
- **Multi-Select**: Unused Docker volumes
- **Multi-Select**: Orphaned Python virtual environments

**Time**: 5-15 minutes | **Recovery**: 1GB - 5GB typically

---

### Aggressive (Yearly/Spring Cleaning)
**Risk Level**: ⭐⭐⭐⭐ High Caution | **Complexity**: ⭐⭐⭐⭐ High

Run sparingly (quarterly or yearly). Requires strong understanding of your system; multi-select is mandatory for high-risk items.

**Includes everything from Deep, plus**:
- Docker aggressive system prune (`docker system prune -a`)
- Selective IDE re-indexing
- System-wide thorough analysis of unused caches
- **Multi-Select Only**: Maven local repository (`~/.m2/repository`) - selective cleanup
- **Multi-Select Only**: Old language version managers (keep only actively used versions)
- **Multi-Select Only**: Large git repository optimization

**Time**: 15-60 minutes | **Recovery**: 5GB - 20GB+ (highly variable)

---

## Cleanup Level Reference Table

| Level | Frequency | Risk | Complexity | Data Loss Risk | Recovery | Multi-Select |
|-------|-----------|------|-----------|-----------------|----------|-------------|
| **Quick** | Daily | Minimal | Very Low | None | 100MB-500MB | None |
| **Standard** | Weekly | Low | Low | None | 500MB-3GB | None |
| **Deep** | Monthly | Medium | Medium | Low | 1GB-5GB | Some menus |
| **Aggressive** | Yearly | High Caution | High | Possible* | 5GB-20GB+ | Mandatory |

*Possible only if misconfigured or important projects depend on old versions.

## Multi-Select Mode vs Force Mode

### Default Behavior: Multi-Select Menus for High-Risk Operations

By default, high-risk operations present a **multi-select menu** where you can choose exactly which items to delete:

```bash
# Default: Shows multi-select menu for high-risk items
upperclean --deep

# Output example:
# Found 3 old Node versions:
#   [ ] node/v14.19.0 (458 MB)
#   [✓] node/v16.13.2 (512 MB)
#   [✓] node/v18.0.0 (525 MB)
#
# Found 2 unused Docker volumes:
#   [✓] volume_abc123 (2.3 GB)
#   [ ] volume_def456 (1.1 GB)
#
# Arrow keys to navigate, Space to select, Enter to confirm
```

**Multi-Select Features:**
- View all items with sizes before deciding
- Select multiple items at once (space bar)
- Navigate with arrow keys
- Toggle individual selections
- Skip entire categories if desired
- See total recovery before confirming

Multi-select menus appear for:
- **Deep level**: Old version managers (nvm, rbenv), unused Docker volumes, orphaned virtual environments
- **Aggressive level**: Maven repository, old language versions, large git optimizations

### Force Mode: Skip Multi-Select Menus

Use `--force` to skip multi-select menus and automatically delete all high-risk items:

```bash
# Force mode: Skips multi-select menu, deletes all high-risk items automatically
upperclean --deep --force

# Aggressive force cleanup (maximum recovery, requires confidence)
upperclean --aggressive --force
```

**Caution**: Use `--force` only when you're confident about what will be deleted. Always preview with `--dry-run` first!

### Recommended Workflows

#### Recommended (Safe) - Best for Most Users
```bash
# 1. Preview what will be deleted
upperclean --deep --dry-run

# 2. Run with multi-select menus to choose items
upperclean --deep
# → Review items, select/deselect, then confirm each menu
```

#### Advanced (Time-saving, requires confidence)
```bash
# 1. Preview everything with dry-run first
upperclean --aggressive --dry-run

# 2. Force cleanup to skip multi-select menus
upperclean --aggressive --force
```

#### Automation (Unattended cleanup for scripts/cron jobs)
```bash
# Use --force to run without multi-select menus
upperclean --standard --force --global
```

#### Extra Cautious (Maximum control)
```bash
# Force multi-select menus even for standard cleanup
upperclean --standard --interactive --global
# → Shows multi-select menu for more granular control
```

### Mode Reference Table

| Mode | Command | Behavior |
|------|---------|----------|
| **Multi-Select (default)** | `upperclean --deep` | Shows multi-select menu - choose which items to delete |
| **Force Mode** | `upperclean --deep --force` | Skips multi-select menus, auto-deletes all high-risk items |
| **Dry Run** | `upperclean --deep --dry-run` | Preview without deleting anything |
| **Interactive Flag** | `upperclean --standard --interactive` | Forces multi-select menus even for safe operations |

### Multi-Select Interaction Example

```bash
$ upperclean --deep
[✓] Quick cleanup completed
[✓] Standard cleanup completed

Now running Deep cleanup - interactive items found:

┌─ Old Node Versions ──────────────────────────────┐
│ [✓] node/v16.13.2 (512 MB)                      │
│ [✓] node/v18.0.0 (525 MB)                       │
│ [ ] node/v20.0.0 (535 MB) - currently active   │
└──────────────────────────────────────────────────┘

┌─ Unused Docker Volumes ──────────────────────────┐
│ [✓] old_db_volume (3.2 GB)                      │
│ [ ] production_backup (2.1 GB)                  │
└──────────────────────────────────────────────────┘

Total to delete: 4.3 GB
[Space] to toggle | [Up/Down] to navigate | [Enter] to confirm | [q] to cancel
```

You have full control to:
- **Keep specific versions**: Node v20.0.0 stays (currently active)
- **Preserve backups**: Skip production_backup
- **Selective deletion**: Only remove what you're comfortable with
- **See totals**: Know exactly how much space you'll free

## Installation

```bash
# Coming soon
```

## Usage

### By Cleanup Level

```bash
# Quick cleanup (daily) - safest, fastest
upperclean --quick

# Standard cleanup (weekly) - recommended default
upperclean --standard

# Deep cleanup (monthly) - more aggressive
upperclean --deep

# Aggressive cleanup (yearly) - maximum recovery, requires caution
upperclean --aggressive
```

### With Additional Options

```bash
# Global cleanup (default is current project)
upperclean --quick --global

# Dry run to preview what would be deleted
upperclean --standard --dry-run

# Force mode: Skip prompts and auto-delete (use with caution!)
upperclean --deep --force

# Project-level cleanup
upperclean --standard

# Combine flags
upperclean --deep --global --dry-run

# Force global cleanup (for scripts/automation)
upperclean --standard --global --force
```

### Default Behavior

```bash
# Runs standard cleanup on current project (safest default)
upperclean
```

## Configuration

Configure which directories and commands to clean via:
- Global config: `~/.config/upperclean/config`
- Project-local config: `./.upperclean-config`

## Safety Guidelines

### Getting Started Safely

1. **Start with Quick**: If you're new to upperclean, start with `upperclean --quick` on your local project
2. **Always use `--dry-run` first**: Preview what will be deleted before committing
3. **Backup critical data**: Before running Deep or Aggressive levels, backup your system
4. **Test on project-level first**: Master project cleanup before running global cleanup

### Safety by Level

- **Quick & Standard**: Safe to run without dry-run or backup (no multi-select menus)
- **Deep**: Use `--dry-run` first; review multi-select menus before confirming
- **Aggressive**: Always use `--dry-run` and backup first; multi-select menus are mandatory

### Best Practices

- Use `--dry-run` to preview everything before deleting: `upperclean --standard --dry-run`
- Run `--quick` or `--standard` regularly (weekly) to prevent buildup
- Use the multi-select menus (default for Deep/Aggressive) to fine-tune deletions
- Keep backups of important projects before Deep/Aggressive cleanup
- Review configuration files to exclude critical directories
- Take advantage of multi-select: easily deselect items you want to keep

### Understanding Multi-Select Menus

Multi-select menus appear by default for high-risk operations (Deep and Aggressive levels):

```bash
# Shows multi-select menu for high-risk items
upperclean --deep

# Preview the multi-select menus without deleting
upperclean --deep --dry-run

# Force multi-select menus even for standard cleanup
upperclean --standard --interactive
```

**Benefits of multi-select:**
- ✓ See all items with exact file sizes
- ✓ Deselect specific items you want to keep
- ✓ Know total space that will be freed
- ✓ Review each category at your own pace
- ✓ Skip entire operation categories if needed

## License

MIT
