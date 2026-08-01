[![ShapeExpressions Gitter chat https://gitter.im/shapeExpressions/Lobby](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/shapeExpressions/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# shex.io
files for the shex.io web site

## Deployment

The webapps at e.g. <https://shex.io/webapps/shex.js/doc/shex-simple.html> are
served from a plain clone of [shex.js](https://github.com/shexSpec/shex.js)
(with a `doc` symlink to `packages/shex-webapp/doc`).

The webpack bundles (`packages/*/doc/webpacks/`) are gitignored on `main`;
`.github/workflows/webpacks.yml` rebuilds them on every push to `main` and
publishes them to the orphan `webpacks` branch. `tools/sync-webpacks.sh`
copies that branch's bundles into the working tree (untracked, so later
`git pull`s never conflict).

One-time setup in the deployment clone, so a bare `git pull` fetches fresh
bundles automatically:

```sh
printf '#!/bin/sh\ntools/sync-webpacks.sh\n' > .git/hooks/post-merge && chmod +x .git/hooks/post-merge
```

(The hook only fires when a pull merges something, which is also the only time
the bundles can change. `tools/sync-webpacks.sh` can always be run by hand,
e.g. after the first clone or if a sync raced a push to `main`.)
