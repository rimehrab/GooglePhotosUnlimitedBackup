name: Zip and Release Module

on:
  push:
    paths:
      - 'Module/**'  # Trigger only on changes within the Module folder
  workflow_dispatch:  # Allows manual run

jobs:
  zip_and_release:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Extract version from module.prop
      id: get_version
      run: |
        version=$(grep '^version=' Module/module.prop | sed -E 's/version=v([0-9.]+)/\1/')
        echo "VERSION=${version}" >> $GITHUB_ENV

    - name: Create ZIP file
      run: |
        mkdir -p dist
        zip -r dist/Module.zip Module/

    - name: Create GitHub Release
      id: create_release
      uses: actions/create-release@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        tag_name: "${{ env.VERSION }}"  # Using the version from module.prop
        release_name: "Google Photos Unlimited Backup"
        draft: false
        prerelease: false

    - name: Upload release asset
      uses: actions/upload-release-asset@v1
      with:
        repo_token: ${{ secrets.GITHUB_TOKEN }}
        release_id: ${{ steps.create_release.outputs.id }}
        asset_path: dist/Module.zip
        asset_name: Module.zip
        asset_content_type: application/zip

    - name: Clean up ZIP file
      run: rm -rf dist/Module.zip