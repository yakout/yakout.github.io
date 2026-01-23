# Local Development Guide

This guide documents the setup and troubleshooting steps for running this Jekyll site locally, especially on Apple Silicon (ARM64) Macs with older Ruby versions.

## Prerequisites

- **Ruby**: 2.6.x (System Ruby on older macOS versions).
- **Bundler**: 1.17.x or newer.

## Local Setup

Due to compatibility issues with Ruby 2.6 and ARM64 architecture, specific configurations are required:

1.  **Force Native Compilation**:
    Older versions of Bundler may attempt to install `x86_64` pre-compiled binaries for gems like `nokogiri`. To fix this, force Bundler to compile from source:
    ```bash
    bundle config force_ruby_platform true
    ```

2.  **Install Dependencies**:
    Install gems into a local directory to avoid permission issues and keep the system clean:
    ```bash
    bundle install --path vendor/bundle
    ```

## Running the Server

To start the local development server and make it accessible across your network (e.g., via Tailscale IP), bind to all interfaces:

```bash
bundle exec jekyll serve --host 0.0.0.0
```

The site will be accessible at `http://127.0.0.1:4000/` locally or `http://<your-tailscale-ip>:4000/` from other devices.

## Troubleshooting

### `nokogiri` or `ffi` Load Errors
If you see errors like `cannot load such file -- nokogiri/nokogiri`, it's usually an architecture mismatch.
- Ensure `ffi` is pinned to `1.15.5` in the `Gemfile`.
- Run `rm -rf vendor/bundle` and re-run the setup steps above.

### GitHub Pages Compatibility
The `Gemfile` is configured to use the `github-pages` gem. This ensures the local Jekyll version (3.9.x) matches the "Classic" GitHub Pages build environment.
