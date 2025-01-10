# Hugo Module: Headings Colorise

This Hugo module applies custom colors to different heading levels (h1, h2, h3, etc.) in your Hugo site.

## Files and Their Purpose

### 1. `package.json`
This file contains metadata about the project, including dependencies and scripts. It also includes configuration for semantic release to automate versioning and changelog generation.

### 2. `.releaserc.json`
This file configures semantic release plugins to analyze commits, generate release notes, update the changelog, and manage versioning.

### 3. `layouts/partials/head/head-end-scripts.html`
This partial template includes the generated CSS file for the headings color scheme in the HTML head section.

### 4. `assets/scss/headings.scss`
This SCSS file defines the color variables for each heading level and applies these colors to the respective HTML heading tags.

## Implementation Steps

1. **Define SCSS Variables**: In `assets/scss/headings.scss`, define color variables for each heading level and apply these colors to the respective heading tags.

    ```scss
    // Define SCSS variables for heading colors
    $h1-color: #ff4081;
    $h2-color: #3f51b5;
    $h3-color: #4caf50;
    $h4-color: #ff9800;
    $h5-color: #607d8b;
    $h6-color: #795548;

    // Apply colors to headings
    h1 {
      color: $h1-color;
    }

    h2 {
      color: $h2-color;
    }

    h3 {
      color: $h3-color;
    }

    h4 {
      color: $h4-color;
    }

    h5 {
      color: $h5-color;
    }

    h6 {
      color: $h6-color;
    }
    ```

2. **Include CSS in HTML**: In `layouts/partials/head/head-end-scripts.html`, include the generated CSS file in the HTML head section.

    ```html
    <!-- Include headings colorise module styles -->
    {{ $style := resources.Get "scss/headings.scss" | resources.ToCSS (dict "targetPath" "assets/css/headings.css") }}
    <link rel="stylesheet" href="{{ $style.RelPermalink }}" media="screen">
    ```

3. **Configure Semantic Release**: Ensure `package.json` and `.releaserc.json` are properly configured for semantic release to automate versioning and changelog updates.

    ```json
    // package.json
    {
      // ...existing code...
      "release": {
        "branches": ["main"],
        "plugins": [
          "@semantic-release/commit-analyzer",
          "@semantic-release/release-notes-generator",
          "@semantic-release/changelog",
          "@semantic-release/npm",
          "@semantic-release/git"
        ]
      },
      "devDependencies": {
        "@semantic-release/changelog": "^6.0.3",
        "@semantic-release/commit-analyzer": "^13.0.1",
        "@semantic-release/git": "^10.0.1",
        "@semantic-release/npm": "^12.0.1",
        "@semantic-release/release-notes-generator": "^14.0.3",
        "semantic-release": "^24.2.1"
      }
    }
    ```

    ```json
    // .releaserc.json
    {
      "branches": ["main"],
      "plugins": [
        "@semantic-release/commit-analyzer",
        "@semantic-release/release-notes-generator",
        "@semantic-release/changelog",
        {
          "changelogFile": "CHANGELOG.md"
        },
        "@semantic-release/npm",
        [
          "@semantic-release/git",
          {
            "assets": ["CHANGELOG.md", "package.json"],
            "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
          }
        ]
      ]
    }
    ```

4. **Commit and Release**: Use semantic release to commit changes and create new releases automatically.

    ```bash
    # Stage all changes
    git add .

    # Commit with a message following the conventional commit format
    git commit -m "feat: initial release"

    # Push the commit to the main branch
    git push origin main

    # Tag the commit with the version number
    git tag v1.0.0

    # Push the tag to the repository
    git push origin v1.0.0
    ```

By following these steps, you can create a Hugo module that applies custom colors to headings and automate the release process using semantic release.

For more information on semantic release, refer to the [official documentation](https://semantic-release.gitbook.io/semantic-release/).

