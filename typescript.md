# typescript

## install

* `npm install -g typescript`

##  github action

* `npm install @actions/core @actions/github`
* within `packages.json` add to the `scripts` section:
```
"scripts": {
  "build": "tsc build src/index.ts"
}
```

## useful commands

* `tsc --init`  # creates tsconfig.json

## references

* https://jeffrafter.com/working-with-github-actions/
* https://typescriptlang.org

