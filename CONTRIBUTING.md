# Contributing

Important notes for contributors regarding Git LFS and large files.

Git LFS migration

- This repository recently migrated many large geospatial and binary assets into Git LFS. The git history was rewritten and branches were force-pushed.
- If you have a local clone from before the migration, please re-clone the repository after installing Git LFS, or reset your clone to the remote `main` branch (forced history rewrite).

Quick steps (recommended):

1. Install Git LFS:

   brew install git-lfs
   git lfs install

2. Re-clone the repo:

   git clone https://github.com/239x1a33a6-cmyk/Dss-Forest.git

3. If you need to recover a pre-migration clone with local changes, stash/uncommit them first and then:

   git fetch --all
   git reset --hard origin/main

Notes
- Make sure git-lfs is installed before pulling; otherwise you'll see pointer files instead of the actual large binaries.
- If you add new large data files, ensure they match the patterns in `.gitattributes` or update `.gitattributes` accordingly.

Environment validation

This repo includes a small helper to validate required environment variables before running the app.

1. Install dependencies (only needed to run the script locally):

```bash
npm install
```

2. Run the env-check script:

```bash
npm run check-env
```

The script looks for these variables by default:

- JWT_SECRET
- NEXT_PUBLIC_GOOGLE_MAPS_API_KEY
- USE_DATABASE (if true, also checks DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASSWORD)

You can create a `.env.local` file in the repository root with values for development. Example:

```
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your-google-key
JWT_SECRET=replace-with-a-long-random-secret
USE_DATABASE=false
```

If the script exits non-zero, it prints which variables are missing.

