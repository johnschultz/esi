# ESI

An Elixir client for Eve Online's Swagger Interface (ESI).


## Fork Maintainer Note
This package is my attempt at updating [bruce/esi](https://github.com/bruce/esi)
to use the latest EVE Swagger Interface (currently 1.7.2 as of this writing). I've
cherry-picked commits from [marcinruszkiewicz/esi](https://github.com/marcinruszkiewicz/esi/commit/4dfb91df60cfa069e04ba3a2687e1dad088abc5b)
and [/bmartin2015/esi](https://github.com/bmartin2015/esi/commit/75a1e2e24c53631dddb6055dad93900e3fbdae2c)
to accomplish this. Thanks to them for their work.

## Installation

Install from [Hex.pm](https://hex.pm/packages/esi):

```elixir
def deps do
  [{:esi, "~> 0.1"}]
end
```

## Documentation

Documentation is available at <https://hexdocs.pm/esi>.

## Synopsis

This package uses the ESI Swagger Specification as published by CCP
Games---plus some custom rules---to generate an Elixir API that
(hopefully) feels more hand-written than code-generated.

You can see what version of the ESI Swagger Specification was used by running:

``` elixir
ESI.API.version()
# => "1.7.2"
```

API functions are available in modules under `ESI.API`. The module grouping is close to,
but not the same as, [the ESI site](https://esi.evetech.net/ui/?version=_latest), staying more in-line
with the URL used for the specific function.

> Note: Module organization may change for the v1.0 release.

### Basic Usage

API functions return an `ESI.Request.t` which can then be processed by passing to
`ESI.request` or `ESI.request!`:

```elixir
ESI.API.Insurance.prices()
|> ESI.request
# => {:ok, result}
```

```elixir
ESI.API.Insurance.prices()
|> ESI.request!
# => result
```

Automatic pagination is also supported if you use `ESI.stream!` instead:

``` elixir
ESI.API.Universe.bloodlines()
|> ESI.stream!
|> Enum.take(3)
```

### Authorization

Currently, pass the `:token` option for those API functions that use
it.

> In the future, `ESI.request`, `ESI.request!`, and `ESI.stream!` will
> support headers passed as arguments and/or loaded from application
> configuration.

## Development

Download ESI's `swagger.json` to `priv/`

Then run:

```
$ rm lib/esi/api.ex lib/esi/api/*.ex
$ mix esi.generate
```

If you are using Elixir version 1.6.0 or above, format the code that was generated with:

```
$ mix format
```

Then:

- Make sure any new modules/functions look like they're named correctly. If they're
  not, see the renaming functions in `ESI.Generator.Function`.
- Update the `README.md` with the current ESI Swagger Specification version number
  in the code example.

## Contributing

Issues and PRs welcome via the project repository at https://github.com/bruce/esi.

## License

See LICENSE.

Please note this package is not supported or endorsed by Eve
Online/CCP Games. It's merely a utility created by an enthusiastic
fan.
