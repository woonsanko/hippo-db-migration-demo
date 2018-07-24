# hippo-db-migration-demo

This is an example Hippo CMS database migration project following the [recipe-for-hippo-db-migration project](https://github.com/woonsanko/recipe-for-hippo-db-migration).

This demo project simply migrates an H2 based repository to another H2 based repository for demonstration purpose.

## Demo Migration Step-by-step

### Step 1: Build and Run the demo and add some content

- Build with ```mvn clean verify```.
- Run the demo project with ```mvn -P cargo.run -Drepo.path=source-storage```.
- Make sure to turn off "Auto-export" feature, not to generate bootstrap files. So, your content change won't be serialized to bootstrap YAML files in source.
- Add or update some news or events documents
- Stop the tomcat.

### Step 2: Prepare the migration subsytem

- In this demo project, the migration subsystem is already prepared in the [recipe-for-hippo-db-migration](recipe-for-hippo-db-migration) subfolder. So, you may copy or prepare files to the folder in the following.
- Follow [Step 3: Copy all the necessary JAR files to lib/ directory](https://github.com/woonsanko/recipe-for-hippo-db-migration#step-3-copy-all-the-necessary-jar-files-to-lib-directory).

### Step 3: Migrate the source-storage to backup-storage

- In the [recipe-for-hippo-db-migration](recipe-for-hippo-db-migration) subfolder, execute the following command:

```bash
$ sh bin/migrate.sh \
  --conf conf/source-repository.xml \
  --backup-conf conf/backup-repository.xml \
  --repo ../source-storage \
  --backup-repo ../backup-storage
```

### Step 4: Validation

- Run the demo project with ```mvn -P cargo.run -Drepo.path=backup-storage``` again.
- Make sure your new or updated content are there after the migration.
