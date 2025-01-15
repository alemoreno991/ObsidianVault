Tags: #bazel #build-system

---
## Extension Overview

Extension files are files ending in `.bzl`. Use a [load statement](https://bazel.build/concepts/build-files#load) to import a symbol from an extension. 

>[!Note]
A macro is a function that instantiates rules. It is useful when a `BUILD` files is getting too repetitive or too complex, as it lets you reuse some code. The function is evaluated as soon as the `BUILD` files is read. 

>[!Note]
A rule is more powerful that a macro. It can access Bazel internals and have full control over what is going on. It may for example pass information to other rules.





## References
- [Bazel - Extension Overview](https://bazel.build/extending/concepts)
- [Starlark language](https://bazel.build/rules/language)
- [Share Variables](https://bazel.build/build/share-variables)

