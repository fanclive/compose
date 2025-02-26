# docker compose ps

<!---MARKER_GEN_START-->
List containers

### Options

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `-a`, `--all` |  |  | Show all stopped containers (including those created by the run command) |
| [`--filter`](#filter) | `string` |  | Filter services by a property (supported filters: status). |
| [`--format`](#format) | `string` | `table` | Format the output. Values: [table \| json] |
| `-q`, `--quiet` |  |  | Only display IDs |
| `--services` |  |  | Display services |
| [`--status`](#status) | `stringArray` |  | Filter services by status. Values: [paused \| restarting \| removing \| running \| dead \| created \| exited] |


<!---MARKER_GEN_END-->

## Description

Lists containers for a Compose project, with current status and exposed ports.
By default, both running and stopped containers are shown:

```console
$ docker compose ps
NAME           COMMAND                  SERVICE   STATUS       PORTS
example-bar-1  "/docker-entrypoint.…"   bar       exited (0)
example-foo-1  "/docker-entrypoint.…"   foo       running      0.0.0.0:8080->80/tcp
```

## Examples

### <a name="format"></a> Format the output (--format)

By default, the `docker compose ps` command uses a table ("pretty") format to
show the containers. The `--format` flag allows you to specify alternative
presentations for the output. Currently, supported options are `pretty` (default),
and `json`, which outputs information about the containers as a JSON array:

```console
$ docker compose ps --format json
[{"ID":"1553b0236cf4d2715845f053a4ee97042c4f9a2ef655731ee34f1f7940eaa41a","Name":"example-bar-1","Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Project":"example","Service":"bar","State":"exited","Health":"","ExitCode":0,"Publishers":null},{"ID":"f02a4efaabb67416e1ff127d51c4b5578634a0ad5743bd65225ff7d1909a3fa0","Name":"example-foo-1","Command":"/docker-entrypoint.sh nginx -g 'daemon off;'","Project":"example","Service":"foo","State":"running","Health":"","ExitCode":0,"Publishers":[{"URL":"0.0.0.0","TargetPort":80,"PublishedPort":8080,"Protocol":"tcp"}]}]
```

The JSON output allows you to use the information in other tools for further
processing, for example, using the [`jq` utility](https://stedolan.github.io/jq/){:target="_blank" rel="noopener" class="_"}
to pretty-print the JSON:

```console
$ docker compose ps --format json | jq .
[
  {
    "ID": "1553b0236cf4d2715845f053a4ee97042c4f9a2ef655731ee34f1f7940eaa41a",
    "Name": "example-bar-1",
    "Command": "/docker-entrypoint.sh nginx -g 'daemon off;'",
    "Project": "example",
    "Service": "bar",
    "State": "exited",
    "Health": "",
    "ExitCode": 0,
    "Publishers": null
  },
  {
    "ID": "f02a4efaabb67416e1ff127d51c4b5578634a0ad5743bd65225ff7d1909a3fa0",
    "Name": "example-foo-1",
    "Command": "/docker-entrypoint.sh nginx -g 'daemon off;'",
    "Project": "example",
    "Service": "foo",
    "State": "running",
    "Health": "",
    "ExitCode": 0,
    "Publishers": [
      {
        "URL": "0.0.0.0",
        "TargetPort": 80,
        "PublishedPort": 8080,
        "Protocol": "tcp"
      }
    ]
  }
]
```

### <a name="status"></a> Filter containers by status (--status)

Use the `--status` flag to filter the list of containers by status. For example,
to show only containers that are running or only containers that have exited:

```console
$ docker compose ps --status=running
NAME           COMMAND                  SERVICE   STATUS       PORTS
example-foo-1  "/docker-entrypoint.…"   foo       running      0.0.0.0:8080->80/tcp

$ docker compose ps --status=exited
NAME           COMMAND                  SERVICE   STATUS       PORTS
example-bar-1  "/docker-entrypoint.…"   bar       exited (0)
```

### <a name="filter"></a> Filter containers by status (--filter)

The [`--status` flag](#status) is a convenient shorthand for the `--filter status=<status>`
flag. The example below is the equivalent to the example from the previous section,
this time using the `--filter` flag:

```console
$ docker compose ps --filter status=running
NAME           COMMAND                  SERVICE   STATUS       PORTS
example-foo-1  "/docker-entrypoint.…"   foo       running      0.0.0.0:8080->80/tcp

$ docker compose ps --filter status=running
NAME           COMMAND                  SERVICE   STATUS       PORTS
example-bar-1  "/docker-entrypoint.…"   bar       exited (0)
```

The `docker compose ps` command currently only supports the `--filter status=<status>`
option, but additional filter options may be added in the future.
