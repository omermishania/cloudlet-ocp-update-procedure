# Cloudlet Openshift Version Update

### 1. Login to the cluster

    oc login <CLUSTERNAME>

  
### 2. Copy the signature file to the installer of the cluster

    
    scp <SIGNATURE_FILE> root@<INSTALLER>

### 3. Apply the signature file

    oc apply -f <PATH_TO_SIGNATURE_FILE>

### 4. Change the sha256 format

**#NOTE:** Save the sha256 value in notepad, we'll call it <sha256_sum_value>.

Change the `-` in the generated value to `:`
for example:

**before:** `sha256-81154f5c03294534e1eaf0319bef7a601134f891689ccede5d705ef659aa8c92`

**after:** `sha256:81154f5c03294534e1eaf0319bef7a601134f891689ccede5d705ef659aa8c92`

### 5. Set environment variables to make your life easier
**#NOTE:** Make sure you change the version from the X's to the right version.

    export LOCAL_REGISTRY='registry.ocp43.prod.sales.lab.tlv.redhat.com'
    export LOCAL_REPOSITORY='ocp4.X.XX/openshift4.X.XX'
  
### 6. Check that the environment variables and the access to the image repository are okay

    curl https://$LOCAL_REGISTRY/v2/$LOCAL_REPOSITORY/tags/list -u admin:redhat

### 7. Finally, Upgrade the cluster

    oc adm upgrade --allow-explicit-upgrade --to-image ${LOCAL_REGISTRY}/${LOCAL_REPOSITORY}@<sha256_sum_value>
    oc edit ImageContentSourcePolicy

**#NOTE:** In this file, change all appearances of the current version to the desired version.
for example:

**before:** */ocp4.3.22/openshift4.3.22

**after:** */ocp4.3.40/openshift4.3.40


**#NOTE:** When updating a major version (4.3 -> 4.4, 4.4 -> 4.5, etc.),    make sure you change the stable channel to the desired major (stable-4.3 -> stable-4.4, etc.) in the GUI.

Created with @daviddaskalo
