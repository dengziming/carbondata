<!--
    Licensed to the Apache Software Foundation (ASF) under one or more 
    contributor license agreements.  See the NOTICE file distributed with
    this work for additional information regarding copyright ownership. 
    The ASF licenses this file to you under the Apache License, Version 2.0
    (the "License"); you may not use this file except in compliance with 
    the License.  You may obtain a copy of the License at
      http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software 
    distributed under the License is distributed on an "AS IS" BASIS, 
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and 
    limitations under the License.
-->


## CLEAN FILES

Clean files command is used to remove the Compacted, Marked For Delete ,In Progress which are stale and partial(Segments which are missing from the table status file but their data is present)
 segments from the store.

 Clean Files Command
   ```
   CLEAN FILES FOR TABLE TABLE_NAME
   ```


### TRASH FOLDER

  Carbondata supports a Trash Folder which is used as a redundant folder where all stale(segments whose entry is not in tablestatus file) carbondata segments are moved to during clean files operation.
  This trash folder is mantained inside the table path and is a hidden folder(.Trash). The segments that are moved to the trash folder are mantained under a timestamp 
  subfolder(each clean files operation is represented by a timestamp). This helps the user to list down segments in the trash folder by timestamp.  By default all the timestamp sub-directory have an expiration
  time of 7 days(since the timestamp it was created) and it can be configured by the user using the following carbon property. The supported values are between 0 and 365(both included.)
   ```
   carbon.trash.retention.days = "Number of days"
   ``` 
  Once the timestamp subdirectory is expired as per the configured expiration day value, that subdirectory is deleted from the trash folder in the subsequent clean files command.

### FORCE DELETE TRASH
The force option with clean files command deletes all the files and folders from the trash folder.

  ```
  CLEAN FILES FOR TABLE TABLE_NAME options('force'='true')
  ```
Since Clean Files operation with force option will delete data that can never be recovered, the force option by default is disabled. Clean files with force option is only allowed when the carbon property ```carbon.clean.file.force.allowed``` is set to true. The default value of this property is false.