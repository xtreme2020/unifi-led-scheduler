## Latest update [1.1] - 2026-01-09
### Changed
- Updated base image to alpine:3.23.2
- Security updates for supercronic from version 0.2.29 to 0.2.41
- No functional changes

### To update
To update to this version, pull the latest image from the repository:

```
Go inside the compose directory
cd /path/to/your/compose/directory
# Pull the latest image
docker compose pull 
docker compose stop
docker compose up -d
```