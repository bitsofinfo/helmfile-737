# helmfile-737

https://github.com/roboll/helmfile/issues/737

IMPORTANT!: This uses `mergeOverwrite`. You must use a build of `helmfile` that incorporates this PR: https://github.com/roboll/helmfile/pull/735

Its also possible by just using `merge` as well (experienced this prior to 735 as well)

## Fails #1:

Fails w/ panic (you may have to run more than once as it fails about 90% of runs w/ panic).

```
./helmfile --log-level debug -f helmfile.fail.yaml -e test4 apply
```

## Fails #2:

Same as above (you may have to run more than once as it fails about 90% of runs w/ panic).

```
./helmfile --log-level debug -f helmfile.fail2.yaml -e test4 apply
```

This block, which has nothing to do w/ any other variables... very weird

(you may have to run more than once as it fails about 90% of runs w/ panic).

```
...
values:
{{ range $f := list "values/test1/values.yaml" "values/test2/values.yaml" "values/test3/values.yaml" }}
- {{$f}}
{{ end }}
```

## Fails #3:

Same as above, even removes all references to `merge` and or `mergeOverwrite`

```
./helmfile --log-level debug -f helmfile.fail3.yaml -e test4 apply
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
