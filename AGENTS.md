# Repository instructions

## Repository layout

- This repository contains the Hugo source and uses the `master` branch.
- The canonical local deployment checkout is `../staubichsauger.github.io`, resolved relative to this repository. Do not assume an absolute user or workspace path.
- The sibling repository contains the generated GitHub Pages site, uses the `main` branch, and publishes to <https://staubichsauger.github.io/>.
- Preserve unrelated changes in both repositories.

## Prerequisites

- Use Hugo Extended because the theme requires it.
- Initialize the theme submodule before building:

  ```sh
  git submodule update --init -- themes/terminal
  ```

- Check both working trees before deployment:

  ```sh
  git status --short --branch
  git -C ../staubichsauger.github.io status --short --branch
  ```

- Do not deploy into a sibling checkout with unreviewed changes. `--cleanDestinationDir` removes files from the generated destination when they are no longer part of the build.

## Build and validate

- For local preview, run:

  ```sh
  hugo server
  ```

- Build the production site into the sibling checkout with:

  ```sh
  hugo --gc --minify --cleanDestinationDir \
    --destination ../staubichsauger.github.io
  ```

- Do not commit `.hugo_build.lock`.
- Inspect the generated changes and validate them before committing:

  ```sh
  git -C ../staubichsauger.github.io status --short
  git -C ../staubichsauger.github.io diff --check
  ```

## Deployment

1. Make and validate the source changes in this repository.
2. Commit and push the source repository's `master` branch.
3. Confirm that `../staubichsauger.github.io` is on `main`, is clean before the build, and is up to date with its remote.
4. Build the exact committed source into the sibling repository using the production command above.
5. Review, commit, and push the generated site:

   ```sh
   git -C ../staubichsauger.github.io add -A
   git -C ../staubichsauger.github.io diff --cached --check
   git -C ../staubichsauger.github.io commit -m "Deploy site"
   git -C ../staubichsauger.github.io push origin main
   ```

6. Check the latest GitHub Pages build:

   ```sh
   gh api repos/staubichsauger/staubichsauger.github.io/pages/builds/latest
   ```

   If pushing `main` does not enqueue a Pages build, trigger one explicitly:

   ```sh
   gh api --method POST \
     repos/staubichsauger/staubichsauger.github.io/pages/builds
   ```

7. Verify the deployed page at <https://staubichsauger.github.io/>.

## Legacy `public` submodule

- `.gitmodules` and `deploy.sh` still refer to the `public` submodule.
- The normal deployment workflow currently uses the sibling checkout at `../staubichsauger.github.io`.
- Do not run `deploy.sh` unless intentionally restoring or using the submodule workflow and its branch and remotes have been verified.
- Do not update the `public` gitlink merely because the sibling checkout was rebuilt.
