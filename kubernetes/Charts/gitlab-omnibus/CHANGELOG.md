**0.1.35**
> Upgrade note: 
* Due to the change in default access mode, existing users will have to specify `ReadWriteMany` as the access mode. For example:
```
gitlabDataAccessMode=ReadWriteMany
gitlabRegistryAccessMode=ReadWriteMany
gitlabConfigAccessMode=ReadWriteMany
```

