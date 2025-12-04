# Personal Website

This is a personal website built with the [al-folio](template.md) Jekyll theme.

## Environment Setup

### Prerequisites
- [Homebrew](https://brew.sh/) (macOS package manager)
- Git

### Initial Setup on New Machine

1. Clone the repository:
```bash
git clone git@github.com:davevanveen/davevanveen.github.io.git
cd davevanveen.github.io
```

2. Install Docker via Colima (lightweight alternative to Docker Desktop):
```bash
brew install colima docker docker-compose
```

3. Configure Docker CLI plugins:
```bash
mkdir -p ~/.docker
echo '{"cliPluginsExtraDirs": ["/opt/homebrew/lib/docker/cli-plugins"]}' > ~/.docker/config.json
```

4. Start Colima:
```bash
colima start
```

## Local Development

### Using Docker (Recommended)

1. Pull the al-folio Docker image and start the server:
```bash
docker compose pull
docker compose up
```

2. On first run, install gems inside the container:
```bash
# In a new terminal, while docker compose up is running:
docker exec davevanveengithubio-jekyll-1 bundle install
docker compose restart
```

3. The site will be available at `http://localhost:8080` after Jekyll finishes building (takes ~1-2 minutes on first run).

4. Changes to files are automatically reloaded.

5. To stop the server, press `Ctrl+C` in the terminal running `docker compose up`.

### Troubleshooting

If you encounter gem dependency errors:
```bash
docker exec davevanveengithubio-jekyll-1 bundle install
docker compose restart
```

### Alias for Convenience

Add to your `.bashrc` or `.zshrc`:
```bash
alias dockup="docker compose up"
alias dockdown="docker compose down"
```

## Deployment

### Automatic Deployment (Active)

The site automatically deploys to GitHub Pages when you push to the `master` branch. GitHub Actions handles the build and deployment process.

**Standard workflow:**
1. Make changes locally and test with Docker
2. Commit changes: `git add . && git commit -m "your message"`
3. Push to GitHub: `git push origin master`
4. GitHub Actions automatically builds and deploys to `gh-pages` branch
5. Site updates at https://davevanveen.com

### Manual Deployment (Alternative)

If needed, you can manually deploy using:
```bash
./bin/deploy
```

This script:
- Builds the Jekyll site with `bundle exec jekyll build --lsi`
- Purges unused CSS
- Pushes to `gh-pages` branch

## Project Structure

- `_config.yml` - Site configuration
- `_pages/` - Page content (about, publications, etc.)
- `_bibliography/` - Publication BibTeX files
- `_news/` - News items for homepage
- `assets/` - Images, PDFs, and other static files
- `docker-compose.yml` - Docker configuration

## Key Commands

```bash
# Start local server
docker compose up

# Pull latest Docker image
docker compose pull

# Rebuild Docker image from scratch
docker compose up --build --force-recreate

# Deploy manually (if needed)
./bin/deploy
```

## Notes

- The site uses Jekyll with the al-folio theme
- Local changes appear at `localhost:8080` when using Docker
- Production builds require `JEKYLL_ENV=production` (handled automatically by deployment)
- See [template.md](template.md) for more information about the al-folio theme
