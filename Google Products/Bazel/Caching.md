Tags: #bazel #build-system #devops

---
Taken from [A babel in Bazel](https://sluongng.hashnode.dev/series/bazel-caching-explained).

# Overview

Bazel is an artifact-first build tool. This means bazel will execute various actions in order to produce a targeted artifact to the user.

![[Pasted image 20240813160517.png]] 
 Each rectangle represents an artifact and each arrow represents an action. 

> ARTIFACT: any blob file that is currently in your workspace (source code, git LFS) or externally downloaded dependencies. An artifact could also be any intermediary generated file.

> ACTION: consumes one or multiple artifacts to produce at least one artifact.

The user interacts with Bazel by telling it what to produce at the end. Bazel then parse up the current workspace configuration and construct a graph of artifacts and actions.

# Hermeticity

Bazel operates under the assumption of hermeticity (i.e given a set of inputs and an action consuming those inputs, the output is deterministic).

> The hermeticity assumption allows one to execute each action only once.

Bazel exploits the hermeticity to generate a **Content Addressable Store** (CAS), i.e. a tree that allows Bazel to retrieve artifacts based on the invariance of the inputs. For this Bazel treats each artifact as a blob (generic binary file). Each blob will get its content hashed by a strong cryptographic algorithm (SHA256), which becomes the *cache-key*. The *cache-value* is the artifact blob, which is stored under a directory where the path is made up of the *cache-key* (this allows Bazel to look for the artifact content by querying its *cache-key*). 

The following diagram depicts the CAS in a visual way. Note that it's very easy to see that the CAS caches the content of each artifact (cache-value) by storing it in a directory named after the hash of it's content (cache-key).
![[Pasted image 20240813165342.png]]

Bazel represents action's through a series of elements referred to as the action metadata (cache-value), which is comprised by:

- *inputs*  
- *action's definition*
- *hashes and names of the outputs* 

The *inputs* and *action's definitions* are hashed and used as the *cache-key* for future look up. These key-value will be stored in the **Action Cache** (AC). 

![[Pasted image 20240813174202.png]]

> NOTE: The Action Cache does not include the output content in the hash calculation. This allows one to look up the output hashes in the cache-value of the AC while knowing only the inputs and action's definitions. Finally, the actual content of the output artifact can then be retrieved from the CAS.

The aforementioned lead to Bazel's speed and repeatability because on a subsequent invocation of an action, the input artifacts' hashes and action's definition will be used to form the action metadata. Before executing the action, Bazel will try to look up the action's outputs' hashes as recorded in the AC. If the outputs's hashes are found in the AC and the output can be found in the CAS using said hashes then Bazel skips out on executing the action. Otherwise, the action is executed and the CAS and AC are updated for future re-use. 

# In Memory Cache

