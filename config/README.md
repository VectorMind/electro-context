# Resolving workspace paths

Project manifests name a root and a relative path. For dev and drive_projects, read the TOML file named by ELECTRO_HOST_CONFIG if set. Otherwise read ~/.config/electro-context/host.toml if present. For each missing root use the value in host.example.toml on Windows. On other hosts set the roots to their local mount points. An explicitly selected missing config is a configuration error. Expand a leading ~ against the current user's home. Resolve the project root as the directory containing project.yaml.

Copy host.example.toml to the user config location to customize a machine. This convention can be followed directly by agents and later by Python tooling using tomllib. No config generation or Python install is needed for intake.
