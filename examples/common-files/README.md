# Sample Global Configuration

There are two samples in this directory.

## Managed file.

The `settings-configmap.yaml` is a `ConfigMap` that contains a standard Maven `settings.xml` file.  The contents of the file itself is not important, but the Dev Spaces capability that it demonstrates is important.

The labels and annotations on this file tell Dev Spaces what to do with the contents:

```
  labels:
    app.kubernetes.io/component: workspaces-config
    app.kubernetes.io/part-of: che.eclipse.org    
    controller.devfile.io/mount-to-devworkspace: "true"
    controller.devfile.io/watch-configmap: "true"
  annotations:
    controller.devfile.io/mount-as: subpath
    controller.devfile.io/mount-path: /home/user/.m2
```

This tells Dev Spaces to mount the content as files under the subpath `/home/user/.m2` in each workspaces.  Updating this one `ConfigMap` in the `openshift-devspaces` namespaces will automatically roll out the new version of the file to each workspace.

Similarly, the `envvar-secret.yaml` file contains key/value pairs that will be mounted as environment variables in each workspace.  The main difference is in the annotations and labels:

```
  labels:
    app.kubernetes.io/component: workspaces-config
    app.kubernetes.io/part-of: che.eclipse.org
    controller.devfile.io/mount-to-devworkspace: 'true'
    controller.devfile.io/watch-secret: 'true'
  annotations:
    controller.devfile.io/mount-as: env
```

Here, the `mount-as` directive is `env`, meaning the key/value pairs in this `Secret` will be automatically injected as environment variables in workspaces.