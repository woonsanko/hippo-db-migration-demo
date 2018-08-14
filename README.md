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
  - You may download jackrabbit-standalone-2.16.2.jar as Hippo CMS 12.3 uses Jackrabbit 2.16.x.
  - You should donwload hippo-addon-checker-2.1.0.jar.
  - And the other files under Tomcat can be found under ```target/tomcat8x/``` folder once you've run ```mvn -P cargo.run```.

### Step 3: Migrate the source-storage to backup-storage

- First, copy the ```source-storage``` folder in the project root to ```recipe-for-hippo-db-migration/``` folder.
- Copy ```recipe-for-hippo-db-migration/source-storage/workspaces/default/workspace.xml``` file to ```recipe-for-hippo-db-migration/source-storage/workspaces/default/workspace-origin.xml```.
- Edit ```recipe-for-hippo-db-migration/source-storage/workspaces/default/workspace.xml``` to keep only ```<FileSystem>``` and ```<PersistenceManager>``` elements by copying those from [recipe-for-hippo-db-migration/conf/source-repository.xml](recipe-for-hippo-db-migration/conf/source-repository.xml). See [Step 2](https://github.com/woonsanko/recipe-for-hippo-db-migration/blob/master/README.md#step-2-copy-the-repository-directory-of-the-source-system-to-local-system-where-you-execute-this-tool) for detail.
- In the [recipe-for-hippo-db-migration](recipe-for-hippo-db-migration) subfolder, execute the following command:

```bash
$ sh bin/migrate.sh \
  --conf conf/source-repository.xml \
  --backup-conf conf/backup-repository.xml \
  --repo source-storage \
  --backup-repo backup-storage
```

- The above command will create ```recipe-for-hippo-db-migration/backup-storage``` folder and copy all database to that folder.

### Step 4: Validation

- Copy ```recipe-for-hippo-db-migration/backup-storage``` folder to ```storage``` folder in the project root folder.
- Remove ```storage/workspaces/default/workspace.xml``` file
  copy the ```source-storage/workspaces/default/workspace-origin.xml``` (which you backup in the earlier step)
  to ```storage/workspaces/default/workspace.xml```.
- Run the demo project with ```mvn -P cargo.run -Drepo.path=storage```.
- Make sure your new or updated content are there after the migration.
