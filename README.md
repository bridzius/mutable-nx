NX Task graph invalid when NX daemon disabled or on CI.

Repo has three apps - app1, app2, app3, that have three custom targets that depend on one another: `one > two > three`. For example, when running `npx nx run app1:one`, tasks would be:
- app1:three
- app1:two
- app1:one

Run 
`NX_DAEMON=false npx nx affected -t one --files=nx.json --skip-nx-cache`

Expectation:
9 targets run:
- app1:three
- app1:two
- app1:one
- app2:three
- app2:two
- app2:one
- app3:three
- app3:two
- app3:one

Actual result:
5 targets run:
- app1:three
- app1:two
- app1:one
- app2:one
- app3:one

Unrelated target of the first affected project is taken as a dependency for the subsequent projects. This is not reproducable when `NX_DAEMON=true`
