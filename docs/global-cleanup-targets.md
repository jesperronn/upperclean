# Global Cleanup Targets

This document outlines all areas that can be safely cleaned up system-wide by the cleanup utility.

## Package Manager Caches

### npm
- **Path**: `~/.npm`
- **Alternative**: `~/.npmrc` cache directory
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 1GB
- **Command**: `npm cache clean --force`
- **Details**: Stores cached npm packages. Safe to clean; npm will re-download as needed.

### Yarn
- **Path**: `~/.yarn/cache`
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 500MB
- **Command**: `yarn cache clean`
- **Details**: Yarn package cache. Safe to clean; will be rebuilt on next install.

### pip (Python)
- **Path**: `~/.cache/pip` (Linux/macOS) or `%LocalAppData%\pip\Cache` (Windows)
- **Safety Level**: Safe
- **Estimated Recovery**: 50MB - 500MB
- **Command**: `pip cache purge`
- **Details**: Python package cache. Safe to remove; pip will re-download on next install.

### RubyGems
- **Path**: `~/.gem` or `~/.cache/gem`
- **Safety Level**: Safe
- **Estimated Recovery**: 50MB - 200MB
- **Command**: `gem cleanup`
- **Details**: Gem cache and old gem versions.

### Homebrew (macOS)
- **Path**: `~/Library/Caches/Homebrew` or `/Library/Caches/Homebrew`
- **Safety Level**: Safe
- **Estimated Recovery**: 200MB - 2GB
- **Command**: `brew cleanup` or `brew cleanup -s` (aggressive)
- **Details**: Homebrew package cache and old bottle downloads.

### APT (Linux - Debian/Ubuntu)
- **Path**: `/var/cache/apt/archives`
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 500MB
- **Command**: `sudo apt clean` or `sudo apt autoclean`
- **Details**: Downloaded .deb files. Safe to clean; can be re-downloaded if needed.

---

## Build Tool Caches

### Gradle
- **Path**: `~/.gradle/caches`
- **Safety Level**: Safe
- **Estimated Recovery**: 500MB - 5GB
- **Command**: `gradle clean` (project-level) or manual removal
- **Details**: Gradle dependency cache. Safe to delete; will be rebuilt on next build.

### Maven Repository
- **Path**: `~/.m2/repository`
- **Safety Level**: Caution Required (Interactive)
- **Estimated Recovery**: 1GB - 10GB+
- **Command**: `rm -rf ~/.m2/repository` (not recommended automatically)
- **Details**: Downloaded Maven artifacts. Large but safe to delete. Should be interactive due to size.
- **Note**: Can safely clean only specific old versions; use `mvn dependency:purge-local-repository` for safer cleanup.

### SBT (Scala Build Tool)
- **Path**: `~/.sbt`
- **Safety Level**: Caution Required
- **Estimated Recovery**: 100MB - 1GB
- **Command**: Manual removal recommended
- **Details**: SBT cache and plugins. Contains compiled artifacts.

### Cargo (Rust)
- **Path**: `~/.cargo/registry/cache` and `~/.cargo/git/db`
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 2GB
- **Command**: `cargo cache --autoclean`
- **Details**: Rust package manager cache. Safe to clean.

---

## Docker

### Dangling Images
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 5GB+
- **Command**: `docker image prune -f`
- **Details**: Removes untagged images created during builds.

### Stopped Containers
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 1GB
- **Command**: `docker container prune -f`
- **Details**: Removes stopped containers. Ensure they're not needed before cleanup.

### Build Cache
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 2GB
- **Command**: `docker builder prune -f`
- **Details**: Clears build cache layers. Safe; will be rebuilt on next build.

### Unused Volumes
- **Safety Level**: Caution Required (Interactive)
- **Estimated Recovery**: Variable (often 100MB - 1GB+)
- **Command**: `docker volume prune -f`
- **Details**: Removes unattached volumes. Must ensure no important data is stored.

### Full System Cleanup
- **Safety Level**: Caution Required (Interactive)
- **Estimated Recovery**: 500MB - 10GB+
- **Command**: `docker system prune -f` or `docker system prune -a`
- **Details**: Comprehensive cleanup. Use `-a` for aggressive cleanup.

---

## IDE & Editor Caches

### VS Code
- **Paths**:
  - Cache: `~/.cache/Code` (Linux) or `~/Library/Application Support/Code/Cache` (macOS)
  - Extensions cache: `~/.vscode/extensions/.cache`
- **Safety Level**: Safe
- **Estimated Recovery**: 100MB - 500MB
- **Details**: Code editor cache. Safe to delete; will be rebuilt on startup.

### IntelliJ IDEA / JetBrains IDEs
- **Paths**:
  - macOS: `~/Library/Caches/JetBrains`
  - Linux: `~/.cache/JetBrains`
  - Windows: `%LocalAppData%\JetBrains`
- **Safety Level**: Safe
- **Estimated Recovery**: 500MB - 2GB
- **Details**: IDE caches and indices. Safe to clean; rebuilt on next startup.

### Vim
- **Path**: `~/.vim/swap` and `~/.vim/undo`
- **Safety Level**: Safe
- **Estimated Recovery**: 10MB - 100MB
- **Details**: Swap and undo files. Safe to clean.

### Sublime Text
- **Path**: `~/.cache/sublime-text-*` or `~/Library/Caches/Sublime Text`
- **Safety Level**: Safe
- **Estimated Recovery**: 50MB - 200MB
- **Details**: Cache files. Safe to clean.

---

## Language-Specific

### Node Version Managers (nvm, fnm)
- **Paths**:
  - nvm: `~/.nvm/versions/node` (old versions)
  - fnm: `~/.local/share/fnm/node-versions` (old versions)
- **Safety Level**: Caution Required (Interactive)
- **Estimated Recovery**: 100MB - 2GB per old version
- **Details**: Old Node versions. Should only delete if no projects depend on them.

### Python Virtual Environments
- **Paths**: Various (project-level `.venv`, `venv`, `env`, etc.)
- **Safety Level**: Caution Required (Project-level only)
- **Estimated Recovery**: 100MB - 1GB per venv
- **Details**: Should be cleaned at project level, not globally.

### Ruby Version Managers
- **Paths**:
  - rbenv: `~/.rbenv/versions` (old versions)
  - rvm: `~/.rvm/rubies` (old versions)
- **Safety Level**: Caution Required (Interactive)
- **Estimated Recovery**: 50MB - 500MB per old version
- **Details**: Old Ruby versions. Only delete if no projects depend on them.

### Java Version Caches
- **Path**: Various (JDK locations, caches)
- **Safety Level**: Caution Required
- **Estimated Recovery**: 100MB - 1GB per version
- **Details**: Requires careful handling; ensure critical versions are retained.

---

## System Temporary Files

### Linux/macOS System Temp
- **Paths**: `/tmp`, `/var/tmp`, `~/Library/Caches` (general cache)
- **Safety Level**: Caution Required (Some files are in-use)
- **Estimated Recovery**: 100MB - 1GB
- **Command**: `rm -rf /tmp/*` (with caution)
- **Details**: Some processes may use /tmp; clean carefully.

### Windows Temp
- **Path**: `%TEMP%` (usually `C:\Users\[User]\AppData\Local\Temp`)
- **Safety Level**: Caution Required
- **Estimated Recovery**: 100MB - 1GB
- **Details**: May contain in-use files; use system cleanup tool.

### Old Log Files
- **Paths**: `/var/log` (system), `~/.local/share/[app]/logs`
- **Safety Level**: Safe (usually)
- **Estimated Recovery**: 100MB - 500MB
- **Details**: Can safely delete old logs; keep recent ones for debugging.

### Crash Dumps
- **Paths**:
  - macOS: `~/Library/Logs/DiagnosticMessages`
  - Linux: Various crash dump locations
- **Safety Level**: Safe
- **Estimated Recovery**: 50MB - 500MB
- **Details**: Diagnostic crash dumps. Safe to clean.

---

## Development-Related

### Git Garbage Collection
- **Path**: Project-level `.git` directory
- **Safety Level**: Safe (Project-level)
- **Estimated Recovery**: 5% - 30% of repo size
- **Command**: `git gc --aggressive` (project-level)
- **Details**: Optimizes git storage. Should be run per-project.

### Git Shallow Clone Cache
- **Path**: `~/.cache/git` (if applicable)
- **Safety Level**: Safe
- **Estimated Recovery**: Variable
- **Details**: Clean cached shallow clones if using aggressive cloning.

### Xcode Derived Data (macOS)
- **Path**: `~/Library/Developer/Xcode/DerivedData`
- **Safety Level**: Safe
- **Estimated Recovery**: 1GB - 10GB+
- **Details**: Build artifacts and intermediate files. Safe to clean; rebuilt on next build.

### Android SDK Cache (Linux/macOS)
- **Paths**: `~/Android/Sdk` (gradle cache within), `~/.gradle` (noted above)
- **Safety Level**: Safe
- **Estimated Recovery**: 500MB - 5GB
- **Command**: `sdkmanager --uninstall [package]` for old SDK versions
- **Details**: Old SDK versions and cache files.

---

## Safety Levels Summary

| Level | Description | Action |
|-------|-------------|--------|
| **Safe** | Can be safely deleted without confirmation | Auto-clean or minimal prompting |
| **Caution Required** | Should be cleaned interactively or with review | User confirmation required |
| **Interactive Only** | Must have user approval per item | Full interactive mode, confirm each file/dir |

---

## Platform-Specific Notes

### macOS
- Use `~/Library/Caches` for most application caches
- Check `~/Library/Logs` for log files
- Xcode derived data is a major space consumer
- Homebrew caches can grow very large

### Linux
- System caches in `/var/cache`
- Temp files in `/tmp` and `/var/tmp`
- Watch out for system-critical cache directories
- Use `apt clean` / `apt autoclean` for package managers

### Windows
- Use `%TEMP%` for temporary files
- System caches in `%LocalAppData%` and `%ProgramData%`
- Recycle Bin (`$Recycle.Bin`) can be included
- Windows can use disk cleanup tools

---

## Recommended Global Cleanup Order

1. **Safe, High Impact**: Docker, npm, pip, yarn, Homebrew
2. **Safe, Medium Impact**: Gradle, IDE caches, Xcode derived data
3. **Caution Required**: Maven repository, old language versions, system temp
4. **Interactive Only**: Unused Docker volumes, language version managers

This order maximizes space recovery while minimizing risk early in the cleanup process.
