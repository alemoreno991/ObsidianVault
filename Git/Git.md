Tags: #git 

---
# Commands

## Cloning

- `git clone --depth=1 <repository-url>` (It only clones the commit at HEAD)
- `git clone --branch=<commit-sha|tag|branch> <repository-url>` (It clones and checkout the specified commit/tag/branch)

## See difference between two branches

- `git diff <branch>` (full description of the changes)
- `git diff --stat <branch>` (summary of files changed)

## See difference between two files

`git diff <filename>` (full description)
`git diff --word-diff <filename>` (a different way to display it)
## Explore a single commit

`git show <commit-id>` (full description)
`git show --stat <commit-id>` (summarized description)

## Fine control of stage

`git add --patch <filepath>` 


# Use Cases

## Very long feature

Suppose you worked on a very difficult and long feature that ended up modifying several files. Furthermore, assume by the time you're done you realize that this BIG feature can be split into smaller features and you can create several PRs for each small feature.

Assume the following case:

```
12b80d8 (HEAD -> split/cost_function) wip
0049e4a Wip
1379b11 copy sphere.dae to src/ and adjust path names - retry (#304)
5e87786 (origin/lmm/313-local-mapper-update) Updating the uri of base images from public to private (#316)
f3352f4 Update CI pipeline to use latest gnc image (#315)
e35aa74 Update .github/workflows/all-checks-passed.yml
2f7c38f Update README issue (#312)
aab8116 (origin/lmm/198-parametrize-config-of-submodules) Remove simple_demo and add test cases to trajectory generation (#310)
6f14a56 First octomap Bazel build (#307)
02c7de4 (origin/lmm/128-write-doxygen-config) Remove old middleware non-unit test (#308)
9184d1d README for the trajectory package (#305)
cc612ba Replace macros with constexpr (#306)
65a7091 Enforce clang-tidy and clang-format rules (#289)
8df4cba Increase allchecks retries number from 10 to 60 (#303)
7fa29c4 Add .github/workflows/all-checks-passed.yml
d0efd8e Remove deprecated tests (#301)
071a2a4 Create SITL for Mavbridge reader (#296)
3293854 Remove the demos folder (#291)
2f5bff0 Fix bug in the full integration test (#292)
a3ee284 Add new test cases to Middleware and implement parameterized tests (#290)
```

Where the first commit of your `big-feature-branch` is `a3ee284`

```shell
git checkout -b split/small-feature-1
git reset a3ee284
```

Now, you can add all the relevant files to the `small-feature-1` 

```shell
# For entire files
git add <filename-1> <filename-2>
```

```shell
# For hunks (chunks of code)
git add -p <filename>
# You'll enter an interactive session where you can select the necessary hunks for the small-feature-1
```

You can test and make sure everything is working before commiting.

```shell
# Once you are done you can clean all the files and directories that were not staged and committed
git clean -fd
```

