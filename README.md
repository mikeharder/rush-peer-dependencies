# rush-peer-dependencies

Rush fails to update or install dependencies when multiple packages attempt to use different versions of a peer dependency.

To repro, clone and run `rush update`, which fails with:

```
ERROR  @azure/amqp-common@1.0.0-preview.6 requires a peer of rhea-promise@^0.1.15 but none was installed.
```

However, I believe the dependencies in this repo are valid.  Both `project1` and `project2` specify matching versions of their dependency and peer dependency:

https://github.com/mikeharder/rush-peer-dependencies/blob/05203c08c2358e6fe7843226cf6cdeffe21eba1f/project1/package.json#L12-L13

https://github.com/mikeharder/rush-peer-dependencies/blob/05203c08c2358e6fe7843226cf6cdeffe21eba1f/project2/package.json#L12-L13

Note that `project1` depends on `core` which is another project in this repo, while `project2` depends on `@azure/amqp-common` which is a package from npmjs.com.  `core` has a peer dependency on package `rhea-promise@^1.0.0`, while `@azure/amqp-common` has a peer dependency on package `rhea-promise@^0.1.5`.

So this issue might only repro if one package with the peer dependency is in the repo, while the other package is pulled from npmjs.com.  I tried to repro with both dependencies in the repo, but this seemed to work fine:

https://github.com/mikeharder/rush-peer-dependencies/tree/peer-dependency-in-own-project
