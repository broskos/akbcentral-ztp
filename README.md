# ztp_policygen

## Step 1:

Configure the siteconfig.yaml , as well as policygentemplate.yaml , and push those into git.

This is the MOST critical and error prone step... so do this with care. 

There is a good way to validate your siteconfig and policygen templates: 

Lets say your siteconfig (along with kustomization.yaml) is placed in `~/ztp_policyget/siteconfig` directory, then to generate CRs from this siteconfig file, use:
```
podman run --rm --log-driver=none -v ~/ztp_policygen/siteconfig:/resources:Z,U ztp-site-generator:latest generator install .

Console Output:
/resources/cluster-ns.yaml does not have SiteConfig CR. Skipping...
/resources/kustomization.yaml does not have SiteConfig CR. Skipping...
/resources/secrets.yaml does not have SiteConfig CR. Skipping...
Processing SiteConfigs: /resources/sno.yaml /resources/sno_146.yaml /resources/sno_172.yaml 
Generating installation CRs into /resources/out/generated_installCRs ...

```
If the siteconfig file is valid, then the CRs generated will be in `~/ztp_policygen/siteconfig/out/generated_installCRs` directory. 

```
ls ~/ztp_policygen/siteconfig/out/generated_installCRs/cwl-sno3/
cwl-sno3_agentclusterinstall_cwl-sno3.yaml          cwl-sno3_klusterletaddonconfig_cwl-sno3.yaml
cwl-sno3_baremetalhost_cwl-sno3.jnpr.bos2.lab.yaml  cwl-sno3_managedcluster_cwl-sno3.yaml
cwl-sno3_clusterdeployment_cwl-sno3.yaml            cwl-sno3_namespace_cwl-sno3.yaml
cwl-sno3_configmap_cwl-sno3.yaml                    cwl-sno3_nmstateconfig_cwl-sno3.jnpr.bos2.lab.yaml
cwl-sno3_infraenv_cwl-sno3.yaml
```

Similiary, to validate your policygen template, use: 
```
podman run --rm --log-driver=none -v ./ztp_policygen/policygentemplates:/resources:Z,U ztp-site-generator:latest generator config .
```

## Step 2:

`cd deployment`

Now run the following script:

`./deploy.sh`

This will take care of everything, and you should be able to access your Argo  using the information provided in the script output. 

## Misc information:
Explanation of the files is as shown ![here](images/deployment.png)
