# DNF Optimization

## Configuration File

To edit dnf configuration file, use a text editor with administrator privileges:

```shell
sudo nano /etc/dnf/dnf.conf
```

## Settings

These settings increase speed and optimize dnf. Copy into **[main]** section:

```plaintext
# Sets "Yes" as the default choice for prompts.
defaultyes=True
# Disables delta RPMs.
deltarpm=False
# Time in seconds for metadata synchronization.
metadata_timer_sync=3600
# Number of simultaneous package downloads.
max_parallel_downloads=10
# Forces DNF to resolve to IPv4 addresses.
ip_resolve=4
# Limits the number of installed kernel versions.
installonly_limit=2
# Prevents the installation of "weak dependencies".
install_weak_deps=False
# Automatically selects the fastest download mirror based on your location.
fastestmirror=True
# Enables color output in the terminal.
color=always
# Replaces the standard progress bar with a small animation.
z_wagon=True
```
After saving these changes, run the following command to clear the cache and apply the new mirror/metadata settings:

```Shell
sudo dnf clean all
```