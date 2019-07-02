# helmfile-737

https://github.com/roboll/helmfile/issues/737

IMPORTANT!: This uses `mergeOverwrite`. You must use a build of `helmfile` that incorporates this PR: https://github.com/roboll/helmfile/pull/735

Its also possible by just using `merge` as well (experienced this prior to 735 as well)

## Fails #1:

It fails most of the time w/ a panic. Once in a while it works, but just run it again and it should fail. i.e. some race condition

```
./helmfile --log-level debug -f helmfile.fail.yaml -e test4 apply
```

## Fails #2:

Fails most of the time w/ panic.

```
./helmfile --log-level debug -f helmfile.fails2.yaml -e test4 apply
```

This block, which has nothing to do w/ any other variables... very weird

```
...
values:
{{ range $f := list "values/test1/values.yaml" "values/test2/values.yaml" "values/test3/values.yaml" }}
- {{$f}}
{{ end }}
```


## Works (example 1):

```
./helmfile --log-level debug -f helmfile.works.yaml -e test4 apply
```

When this block is removed the panic stops:
```
{{ range $valueBase := $chartConfigs.appdeploy.chartValues.baseValues }}
- {{$chartConfigs.appdeploy.chartValues.baseValuesRootDir}}/values/{{$valueBase}}/values.yaml
{{ end }}
```

## This also works (template, but not apply)
```
./helmfile --log-level debug -f helmfile.fail.yaml -e test4 template
```
