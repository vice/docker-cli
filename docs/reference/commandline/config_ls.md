# config ls

<!---MARKER_GEN_START-->
List configs

### Aliases

`docker config ls`, `docker config list`

### Options

| Name                                   | Type     | Default | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
|:---------------------------------------|:---------|:--------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`-f`](#filter), [`--filter`](#filter) | `filter` |         | Filter output based on conditions provided                                                                                                                                                                                                                                                                                                                                                                                           |
| [`--format`](#format)                  | `string` |         | Format output using a custom template:<br>'table':            Print output in table format with column headers (default)<br>'table TEMPLATE':   Print output in table format using the given Go template<br>'json':             Print in JSON format<br>'TEMPLATE':         Print output using the given Go template.<br>Refer to https://docs.docker.com/go/formatting/ for more information about formatting output with templates |
| `-q`, `--quiet`                        |          |         | Only display IDs                                                                                                                                                                                                                                                                                                                                                                                                                     |


<!---MARKER_GEN_END-->

## Description

Run this command on a manager node to list the configs in the swarm.

For detailed information about using configs, refer to [store configuration data using Docker Configs](https://docs.docker.com/engine/swarm/configs/).

> **Note**
>
> This is a cluster management command, and must be executed on a swarm
> manager node. To learn about managers and workers, refer to the
> [Swarm mode section](https://docs.docker.com/engine/swarm/) in the
> documentation.

## Examples

```console
$ docker config ls

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
9u9hk4br2ej0wgngkga6rp4hq   my_config                   5 weeks ago         5 weeks ago
mem02h8n73mybpgqjf0kfi1n0   test_config                 3 seconds ago       3 seconds ago
```

### <a name="filter"></a> Filtering (-f, --filter)

The filtering flag (`-f` or `--filter`) format is a `key=value` pair. If there is more
than one filter, then pass multiple flags (e.g., `--filter "foo=bar" --filter "bif=baz"`)

The currently supported filters are:

- [id](#id) (config's ID)
- [label](#label) (`label=<key>` or `label=<key>=<value>`)
- [name](#name) (config's name)

#### id

The `id` filter matches all or prefix of a config's id.

```console
$ docker config ls -f "id=6697bflskwj1998km1gnnjr38"

ID                          NAME                        CREATED             UPDATED
6697bflskwj1998km1gnnjr38   q5s5570vtvnimefos1fyeo2u2   6 weeks ago         6 weeks ago
```

#### label

The `label` filter matches configs based on the presence of a `label` alone or
a `label` and a value.

The following filter matches all configs with a `project` label regardless of
its value:

```console
$ docker config ls --filter label=project

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

The following filter matches only services with the `project` label with the
`project-a` value.

```console
$ docker config ls --filter label=project=project-a

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

#### name

The `name` filter matches on all or prefix of a config's name.

The following filter matches config with a name containing a prefix of `test`.

```console
$ docker config ls --filter name=test

ID                          NAME                        CREATED             UPDATED
mem02h8n73mybpgqjf0kfi1n0   test_config                 About an hour ago   About an hour ago
```

### <a name="format"></a> Format the output (--format)

The formatting option (`--format`) pretty prints configs output
using a Go template.

Valid placeholders for the Go template are listed below:

| Placeholder  | Description                                                                          |
|--------------|--------------------------------------------------------------------------------------|
| `.ID`        | Config ID                                                                            |
| `.Name`      | Config name                                                                          |
| `.CreatedAt` | Time when the config was created                                                     |
| `.UpdatedAt` | Time when the config was updated                                                     |
| `.Labels`    | All labels assigned to the config                                                    |
| `.Label`     | Value of a specific label for this config. For example `{{.Label "my-label"}}`       |

When using the `--format` option, the `config ls` command will either
output the data exactly as the template declares or, when using the
`table` directive, will include column headers as well.

The following example uses a template without headers and outputs the
`ID` and `Name` entries separated by a colon (`:`) for all images:

```console
$ docker config ls --format "{{.ID}}: {{.Name}}"

77af4d6b9913: config-1
b6fa739cedf5: config-2
78a85c484f71: config-3
```

To list all configs with their name and created date in a table format you
can use:

```console
$ docker config ls --format "table {{.ID}}\t{{.Name}}\t{{.CreatedAt}}"

ID                  NAME                      CREATED
77af4d6b9913        config-1                  5 minutes ago
b6fa739cedf5        config-2                  3 hours ago
78a85c484f71        config-3                  10 days ago
```

## Related commands

* [config create](config_create.md)
* [config inspect](config_inspect.md)
* [config rm](config_rm.md)
