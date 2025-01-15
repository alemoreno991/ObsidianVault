Tags: #bazel #lint-format #toolchain #devops

---

The [rules_lint](https://github.com/aspect-build/rules_lint) integrates linting and formatting as first-class concepts under Bazel. The use [clang-format](https://clang.llvm.org/docs/ClangFormat.html) and [clang-tidy](https://clang.llvm.org/extra/clang-tidy/)|(which is what we already use).

Features:

- **No changes needed to rulesets**. Works with the Bazel rules you already use.
- **No changes needed to BUILD files**. You don't need to add lint wrapper macros, and lint doesn't appear in `bazel query` output. Instead, users simply lint their existing `*_library` targets.
- **Incremental**. Lint checks (including producing fixes) are run as normal Bazel actions, which means they support Remote Execution and the outputs are stored in the Remote Cache.
- **Can lint changes only**. It's fine if your repository has a lot of existing issues. It's not necessary to fix or suppress all of them to start linting new changes. This is sometimes called the "Water Leak Principle": you should always fix a leak before mopping the spill.
- **Can format files not known to Bazel**. Formatting just runs directly on the file tree. No need to create `sh_library` targets for your shell scripts, for example.
- Honors the same **configuration files** you use for these tools outside Bazel (e.g. in the editor)