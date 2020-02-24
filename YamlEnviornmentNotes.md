# Environments

You can interpolate environment variables from *running docker-compose* using
${VARNAME}, as in:

```yaml
services:
  ghost:
    image:ghost:${GHOST_VERSION}
```

and

```ruby
GHOST_VERSION=2 docker-compose up
```

You can also combine multiple docker-compose files with

```ruby
docker-compose -f compose1.yml compose2.yml
```

And can also use above to run a different dockerfile

# -p flag to change project name
docker-compose also takes a `-p` flag to change the name of the project, which
defaults to the folder you're in. This lets you run a full set of services
side by side to compare

He also mentions YAML templates for reusing code
