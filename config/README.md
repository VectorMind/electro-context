# Resolving workspace paths

Project manifests name a root and a relative path. For dev and drive_projects, read the TOML file named by ELECTRO_HOST_CONFIG if set. Otherwise read ~/.config/electro-context/host.toml if present. For each missing root use the value in host.example.toml on Windows. On other hosts set the roots to their local mount points. An explicitly selected missing config is a configuration error. Expand a leading ~ against the current user's home. Resolve the project root as the directory containing project.yaml.

Copy host.example.toml to the user config location to customize a machine. This convention can be followed directly by agents and later by Python tooling using tomllib. No config generation or Python install is needed for intake.

## Checking dependencies

The Windows defaults are `C:/dev` and `~/My Drive/Projects`. See [host.example.toml](host.example.toml) for the format to use when either root differs on your machine.

When starting a handoff, resolve its manifest entries and check the required paths and files. A repository entry may provide a `clone_url` for restoring its checkout. An empty device Drive folder is valid until the task requires a specific evidence file. Create the project cache on first use and check planned outputs when their owning task produces them.
