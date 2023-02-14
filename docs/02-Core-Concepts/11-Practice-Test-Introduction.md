# Practice Test Introduction

In this section, we will take a look at practice test demo.
- Take me to [Video Tutorial](https://kodekloud.com/topic/practice-test-introduction-2/)

- `metadata`
  We cannot have arbitrary fields the `metadata` section in a `yaml` file. Following are possible fields: 
  - `name`: the name of the object
  - `namespace`: the namespace the object belongs to
  - `labels`: key-value pairs used for selecting and grouping objects
  - `annotations`: key-value pairs used for attaching arbitrary metadata to objects
  - `ownerReferences`: identifies the owning object of this object, creating a parent-child relationship
  - `finalizers`: an array of strings used to specify the finalizers that need to be run before the object is deleted.
- `labels`
  `labels` contains a bunch of `key: value` pairs. Under this section, you can have any `key: value` pairs as you want. The name for key has no limit.

## Possible Readings
- [Working with Kubernetes Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/)
