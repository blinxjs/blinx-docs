# Configuring Modules

There are two types of config which we need to handle for a module. One which contains the default configuration of the module, other which the consumer of the module can pass to the module to customise it.

Format for Default Config

```
{
    "modules": [],
    "placeholders": {},
    "listensTo": []
}
```

Format for Supplied Config

```
{
    moduleName: "",
    moduleConfig: {
        container: "",
        placeholders: {
        },
        listensTo: []
    },
    module: moduleObj
}
```



